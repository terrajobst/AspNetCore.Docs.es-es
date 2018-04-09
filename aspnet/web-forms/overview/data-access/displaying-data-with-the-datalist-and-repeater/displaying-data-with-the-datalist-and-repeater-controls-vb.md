---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: Mostrar datos con los controles de repetidor (VB) y DataList | Documentos de Microsoft
author: rick-anderson
description: En los tutoriales anteriores hemos utilizado el control GridView para mostrar los datos. A partir de este tutorial, veremos la creación de modelos informes comunes con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 6aa7cb76295d18711d88dd9855b43b259b558060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>Mostrar datos con los controles de repetidor (VB) y DataList
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe) o [descarga de PDF](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> En los tutoriales anteriores hemos utilizado el control GridView para mostrar los datos. A partir de este tutorial, veremos la creación de modelos informes comunes con los controles DataList y repetidor, a partir de los conceptos básicos de mostrar los datos con estos controles.


## <a name="introduction"></a>Introducción

En todos los ejemplos a lo largo de los últimos 28 tutoriales, si se necesita mostrar varios registros de un origen de datos se convierte en el control GridView. GridView representa una fila para cada registro del origen de datos, mostrar los campos de datos de registro s en columnas. Aunque el control GridView hace un complemento para mostrar, paginar, ordenar, editar y eliminar datos, su aspecto es un poco cuadros. Además, el marcado que se encarga de la estructura de s de GridView es fija incluye HTML `<table>` con una fila de tabla (`<tr>`) para cada registro y una celda de tabla (`<td>`) para cada campo.

Para proporcionar un mayor grado de personalización de la apariencia y el marcado representado al mostrar varios registros, ASP.NET 2.0 ofrece los controles DataList y repetidor (que también estaban disponibles en la versión de ASP.NET 1.x). Los controles DataList y repetidor representan su contenido con plantillas en lugar de BoundFields, CheckBoxFields, ButtonFields y así sucesivamente. Al igual que el control GridView, el control DataList se representa como HTML `<table>`, pero se permite para los datos de varios registros de origen que se mostrarán por cada fila de la tabla. Repetidor, por otro lado, representa ningún marcado adicional que la explícitamente especifique y es un candidato ideal cuando se necesita un control preciso sobre el marcado que se genera.

En los tutoriales docenas o por lo que a continuación, analizaremos crear patrones comunes de creación de informes con los controles DataList y repetidor, a partir de los conceptos básicos de mostrar los datos con estas plantillas de controles. Veremos cómo dar formato a estos controles, cómo modificar el diseño de los registros de origen de datos en DataList, escenarios de maestra/detalles comunes, formas para modificar y eliminar datos, cómo desplazarse a través de registros y así sucesivamente.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Paso 1: Agregar el DataList y páginas Web del Tutorial repetidor

Antes de empezar este tutorial, permiten s primero Tómese un momento para agregar las páginas ASP.NET que se necesitará para este tutorial y los tutoriales de pocos siguientes trabaja con mostrar datos mediante el DataList y repetidor. Empiece por crear una nueva carpeta en el proyecto denominado `DataListRepeaterBasics`. A continuación, agregue las siguientes páginas ASP.NET cinco a esta carpeta, todas ellas configurada para usar la página maestra con `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Cree una carpeta DataListRepeaterBasics y agregar las páginas ASP.NET Tutorial](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**Figura 1**: crear un `DataListRepeaterBasics` carpeta y agregar las páginas ASP.NET Tutorial


Abra la `Default.aspx` página y arrastre el `SectionLevelTutorialListing.ascx` Control de usuario desde el `UserControls` carpeta a la superficie de diseño. Este Control de usuario, que hemos creado en el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-vb.md) tutorial, enumera el mapa del sitio y muestra los tutoriales de la sección actual en una lista con viñetas.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**Figura 2**: agregar la `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))


Para disponer de la pantalla de lista con viñetas los tutoriales de DataList y repetidor que se va a crear, es necesario agregarlos a la asignación de sitio. Abra la `Web.sitemap` de archivos y agregue el siguiente marcado después de las marcas de nodo de asignación de sitio de agregar botones de personalizado:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]


![Actualizar el mapa del sitio para incluir las nuevas páginas ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**Figura 3**: actualizar el mapa del sitio para incluir las nuevas páginas ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Paso 2: Mostrar la información de producto con el control DataList

Similar a FormView, el control DataList s salida representada depende de las plantillas en lugar de BoundFields, CheckBoxFields y así sucesivamente. A diferencia de FormView, el control DataList está diseñado para mostrar un conjunto de registros en lugar de a un solitario. Permiten s empezar este tutorial echando un vistazo a la información de producto de enlace a un control DataList. Comience abriendo la `Basics.aspx` página en el `DataListRepeaterBasics` carpeta. A continuación, arrastre a un control DataList desde el cuadro de herramientas hasta el diseñador. Como se muestra en la figura 4, antes de especificar las plantillas de DataList s, el diseñador muestra como un cuadro gris.


[![Arrastre al control DataList en el cuadro de herramientas hasta el diseñador](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**Figura 4**: arrastre el DataList desde el cuadro de herramientas en el diseñador ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))


Desde el control DataList s etiqueta inteligente, agregue un nuevo ObjectDataSource y configurarlo de modo que use la `ProductsBLL` clase s. `GetProducts` método. Puesto que re crear a un control DataList de solo lectura en este tutorial, establecemos la lista desplegable en (ninguno) en la s de Asistente para insertar, actualizar y eliminar las fichas.


[![Optar por crear un nuevo origen ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**Figura 5**: participar para crear un nuevo origen ObjectDataSource ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))


[![Configurar el ObjectDataSource para utilizar la clase ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**Figura 6**: configurar el ObjectDataSource que se utilizan los `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))


[![Recuperar información sobre todos los productos con el método GetProducts](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**Figura 7**: recuperar información acerca de todos los de los productos mediante la `GetProducts` método ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))


Después de configurar el ObjectDataSource y asociarlo con el control DataList mediante su etiqueta inteligente, Visual Studio creará automáticamente un `ItemTemplate` en el control DataList que muestra el nombre y valor de cada campo de datos devuelto por el origen de datos (vea el a continuación de marcado). Este valor predeterminado `ItemTemplate` apariencia s es idéntico de las plantillas se crean automáticamente cuando se enlaza un origen de datos a la FormView a través del diseñador.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> Recuerde que al enlazar un origen de datos a un control FormView a través de la etiqueta inteligente de FormView s, Visual Studio creó un `ItemTemplate`, `InsertItemTemplate`, y `EditItemTemplate`. Con el control DataList, sin embargo, solo un `ItemTemplate` se crea. Esto es porque el control DataList no tiene integrado de la misma edición e inserción de compatibilidad que ofrece el FormView. DataList contener eventos relacionados con la edición y la eliminación, y edición y eliminación de soporte técnico no pueden agregarse con un poco de código, pero hay s ninguna compatibilidad de cuadro simple como con FormView. Veremos cómo incluir edición y eliminación de compatibilidad con el control DataList en un tutorial posterior.


Permiten s Tómese un momento para mejorar el aspecto de esta plantilla. En lugar de mostrar todos los campos de datos, permiten s mostrar únicamente el nombre del producto s, proveedor, categoría, cantidad por unidad y el precio unitario. Además, permiten s mostrar el nombre de un `<h4>` encabezado y diseñar los campos restantes mediante un `<table>` bajo el encabezado.

Para realizar estos cambios, puede use las características en el Diseñador de control DataList s inteligente etiqueta haga clic en el vínculo Editar plantillas o puede modificar la plantilla manualmente a través de la sintaxis declarativa de s de página de edición de plantillas. Si utiliza la opción de editar plantillas en el diseñador, el código resultante no puede coincidir con el siguiente marcado exactamente, pero cuando se ven a través de un explorador debe ser muy similar a la pantalla se muestra en la figura 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> En el ejemplo anterior se usa controles de Web Label cuya `Text` propiedad se asigna el valor de la sintaxis de enlace de datos. Como alternativa, se pueden ha omitido las etiquetas, escriba en la sintaxis de enlace de datos. Es decir, en lugar de usar `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` podríamos haber en su lugar utilizado la sintaxis declarativa `<%# Eval("CategoryName") %>`.


Dejar en los controles Web Label, sin embargo, se proporcionan dos ventajas. En primer lugar, proporciona una forma más fácil para dar formato a los datos basándose en los datos, como se verá en el siguiente tutorial. En segundo lugar, la opción de editar plantillas en la sintaxis de enlace de datos declarativo de diseñador t mostrar que aparece fuera de cierto control Web. En su lugar, la interfaz de editar plantillas está diseñada para facilitar el trabajo con marcado estático y Web controles y se da por supuesto que cualquier enlace de datos se realizará mediante el cuadro de diálogo Editar DataBindings, que es accesible desde las etiquetas inteligentes de controles Web.

Por lo tanto, cuando se trabaja con el control DataList, que proporciona la opción de modificar las plantillas a través del diseñador, prefiero usar controles de etiqueta Web para que el contenido sea accesible a través de la interfaz de editar plantillas. Como se verá en breve, repetidor requiere que el contenido de plantilla s editarse desde la vista del origen. Por lo tanto, cuando se encarga de crear las plantillas de s repetidor a menudo podrá omitir la etiqueta Web controla a menos que se puede saber que necesitaré para dar formato a la apariencia de los datos enlazados texto basándose en la lógica de programación.


[![Cada producto s salida es representan utilizando el control DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**Figura 8**: cada producto salida es representar utilizando el control DataList s `ItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Paso 3: Mejorar la apariencia del control DataList

Al igual que el control GridView, DataList ofrece una serie de propiedades relacionadas con el estilo, como `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`, y así sucesivamente. Cuando se trabaja con los controles GridView y DetailsView, hemos creado los archivos de máscaras en el `DataWebControls` tema que predefinidos el `CssClass` propiedades de estos dos controles y `CssClass` propiedad por varias de sus subpropiedades (`RowStyle` `HeaderStyle`, y así sucesivamente). Permiten s hacer lo mismo para el control DataList.

Como se describe en el [mostrar datos con el ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) tutorial, un archivo de máscara especifica las propiedades predeterminadas de relativa al aspecto de un control Web; un tema es una colección de archivos de máscara, CSS, imágenes y JavaScript que definen un determinado aspecto y diseño de un sitio Web. En el *mostrar datos con el ObjectDataSource* tutorial, creamos un `DataWebControls` tema (que se implementa como una carpeta dentro de la `App_Themes` carpeta) en la actualidad, que tiene dos archivos de máscaras - `GridView.skin` y `DetailsView.skin`. Permiten s Agregue un tercer archivo de máscara para especificar la configuración de estilo previamente definido para el control DataList.

Para agregar un archivo de máscara, haga doble clic en el `App_Themes/DataWebControls` carpeta, elija Agregar un nuevo elemento y seleccione la opción de archivo de máscara de la lista. Asigne al archivo el nombre `DataList.skin`.


[![Cree un nuevo archivo de máscara denominado DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**Figura 9**: crear una nueva máscara de archivo con nombre `DataList.skin` ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))


Utilice el siguiente marcado para el `DataList.skin` archivo:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

Esta configuración de asigna las mismas clases CSS a las propiedades de DataList adecuadas tal como se utiliza con los controles GridView y DetailsView. Las clases CSS que se usa aquí `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`, y así sucesivamente se definen en el `Styles.css` de archivos y se agregaron en los tutoriales anteriores.

Con la adición de este archivo de máscara, se actualiza la apariencia de s DataList en el diseñador (puede que tenga que actualizar la vista de diseñador para ver los efectos del nuevo archivo de máscara; en el menú Ver, elija la actualización). Como se muestra en la figura 10, cada producto alternas tiene un color de fondo de color rosa claro.


[![Cree un nuevo archivo de máscara denominado DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**Figura 10**: crear una nueva máscara de archivo con nombre `DataList.skin` ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Paso 4: Explorar el DataList s otras plantillas

Además el `ItemTemplate`, el control DataList admite seis otras plantillas opcionales:

- `HeaderTemplate` Si se proporciona, agrega una fila de encabezado a la salida y se usa para representar esta fila
- `AlternatingItemTemplate` usa para representar elementos alternos
- `SelectedItemTemplate` usa para representar el elemento seleccionado; el elemento seleccionado es el elemento cuyo índice se corresponde con el control DataList s [ `SelectedIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` utilizado para representar el elemento que se está editando
- `SeparatorTemplate` Si se proporciona, se agrega un separador entre cada elemento y se usa para representar este separador
- `FooterTemplate` -Si se proporciona, se agrega una fila de pie de página a la salida y se usa para representar esta fila

Al especificar el `HeaderTemplate` o `FooterTemplate`, DataList agrega una fila de encabezado o pie de página adicional a la salida representada. Al igual que con el control GridView s encabezado y pie filas, el encabezado y pie de página en un control DataList no están enlazados a datos. Por lo tanto, ninguna sintaxis de enlace de datos en el `HeaderTemplate` o `FooterTemplate` que intenta obtener acceso a datos enlazados devolverá una cadena vacía.

> [!NOTE]
> Como vimos en el [mostrar información de resumen en el pie de página de GridView e](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) tutorial, mientras que las filas de encabezado y pie de página don sintaxis de enlace de datos de soporte técnico de t, información específica de datos se puede insertar directamente en estas filas desde el GridView s `RowDataBound` controlador de eventos. Esta técnica se puede usar para ambos calculan totales acumulados u otra información de los datos enlazados al control, así como asignar esa información del pie de página. Este mismo concepto se puede aplicar a los controles DataList y repetidor; la única diferencia es que para los DataList y repetidor de creación de un controlador de eventos para el `ItemDataBound` evento (en lugar de para el `RowDataBound` evento).


En nuestro ejemplo, permiten s tienen el título de la información de producto que se muestra en la parte superior del DataList s da como resultado un `<h3>` encabezado. Para ello, agregue un `HeaderTemplate` con el formato adecuado. En el diseñador, esto puede realizarse, al hacer clic en el vínculo Editar plantillas en la etiqueta inteligente de DataList s, elección de la plantilla de encabezado de la lista desplegable y escriba el texto después de seleccionar la opción de encabezado 3 en el estilo de lista desplegable lista (consulte la figura 11).


[![Agregar un elemento HeaderTemplate con la información de producto de texto](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**Figura 11**: agregar un `HeaderTemplate` con la información de producto de texto ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))


Como alternativa, se pueden agregar mediante declaración escribiendo el siguiente marcado dentro de la `<asp:DataList>` etiquetas:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

Para agregar un poco de espacio entre cada lista de productos, permiten s Agregar una `SeparatorTemplate` que incluye una línea entre cada sección. La etiqueta de la regla horizontal (`<hr>`), agrega un divisor de este tipo. Crear el `SeparatorTemplate` para que tenga el siguiente marcado:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> Al igual que el `HeaderTemplate` y `FooterTemplates`, el `SeparatorTemplate` no está enlazado a todos los registros del origen de datos y, por tanto, no puede ser directamente el origen de datos enlazados al control DataList de registros de acceso.


Después de realizar esta adición, al ver la página a través de un explorador debe ser similar a la figura 12. Tenga en cuenta la fila de encabezado y la línea entre cada lista de productos.


[![El control DataList incluye una fila de encabezado y una regla Horizontal entre cada lista de productos](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**Figura 12**: DataList incluye una fila de encabezado y un Horizontal regla entre cada listado de productos ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Paso 5: Representar marcado específico con el Control de repetidor

Si tiene un origen de la vista desde el explorador al visitar el ejemplo DataList de figura 12, verá que el control DataList emite HTML `<table>` que contiene una fila de tabla (`<tr>`) con una celda de tabla única (`<td>`) para cada elemento asociado a la DataList. Esta salida, de hecho, es idéntica a lo que se genera desde un control GridView con TemplateField único. Como veremos en un tutorial posterior, el control DataList permitir mayor personalización de la salida, lo que nos permite mostrar varios registros de origen de datos por cada fila de la tabla.

¿Qué ocurre si no le t desea emitir HTML `<table>`, aunque? Para un control total y completado sobre el marcado generado por un control Web de datos, debemos usar el control de repetidor. Al igual que el control DataList, repetidor se construye basándose en plantillas. Sin embargo, el repetidor solo ofrece las siguientes plantillas de cinco:

- `HeaderTemplate` Si se proporciona, agrega el marcado especificado antes de los elementos
- `ItemTemplate` usada para representar elementos
- `AlternatingItemTemplate` Si se proporciona, se usa para representar elementos alternos
- `SeparatorTemplate` Si se proporciona, agrega el marcado especificado entre cada elemento
- `FooterTemplate` -Si se proporciona, se agrega el marcado especificado después de los elementos

En ASP.NET 1.x, repetidor control se usa normalmente para mostrar una lista con viñetas cuyos datos proceden de un origen de datos. En tal caso, el `HeaderTemplate` y `FooterTemplates` contendría la apertura y cierre `<ul>` etiquetas, respectivamente, mientras el `ItemTemplate` contendría `<li>` elementos con la sintaxis de enlace de datos. Este enfoque todavía se utiliza en ASP.NET 2.0 como vimos en dos ejemplos de la [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-vb.md) tutorial:

- En la `Site.master` página maestra, se usó un repetidor para mostrar una lista con viñetas del contenido del mapa de sitio de nivel superior (informes básicos, informes de filtrado, Customized Formatting y así sucesivamente); otro repetidor anidado utilizado para mostrar las secciones de los elementos secundarios de la secciones de nivel superior
- En `SectionLevelTutorialListing.ascx`, un repetidor utilizado para mostrar una lista con viñetas de las secciones de los elementos secundarios de la sección de asignación de sitio actual

> [!NOTE]
> ASP.NET 2.0 presenta la nueva [control BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), que se puede enlazar a un control de origen de datos para mostrar una lista con viñetas simple. Con el control BulletedList no necesitamos especificar cualquiera de HTML asociada con la lista; en su lugar, simplemente se indica el campo de datos para mostrar como el texto para cada elemento de lista.


Repetidor actúa como una instrucción catch todos los datos de control Web. Si no hay un control existente que genera el marcado que sea necesario, puede utilizarse el control de repetidor. Para ilustrar el uso del repetidor, permiten s tenga la lista de categorías mostrado sobre el control DataList de información de producto creado en el paso 2. En concreto, permiten s tiene las categorías que se muestran en una sola fila HTML `<table>` con cada categoría que se muestra como una columna en la tabla.

Para ello, inicie, arrastre un control de repetidor desde el cuadro de herramientas hasta el diseñador, por encima del control DataList de información de producto. Al igual que con DataList, repetidor muestra inicialmente como un cuadro gris hasta que se ha definido su plantilla.


[![Agregar un repetidor al diseñador](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**Figura 13**: agregar un repetidor al diseñador ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))


Solo una opción de allí s en la etiqueta inteligente de repetidor s: Elegir origen de datos. Optar por crear un nuevo origen ObjectDataSource y configúrelo para utilizar el `CategoriesBLL` clase s. `GetCategories` método.


[![Crear un nuevo origen ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**Figura 14**: crear un nuevo origen ObjectDataSource ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))


[![Configurar el ObjectDataSource para utilizar la clase CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**Figura 15**: configurar el ObjectDataSource que se utilizan los `CategoriesBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))


[![Recuperar información acerca de todas las categorías con el método GetCategories](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**Figura 16**: recuperar información acerca de todos los de las categorías mediante el `GetCategories` método ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))


A diferencia de DataList, Visual Studio no crea automáticamente un ItemTemplate para repetidor después de enlazar a un origen de datos. Además, las plantillas de repetidor s no se puede configurar a través del diseñador y se deben especificar mediante declaración.

Para mostrar las categorías como una sola fila `<table>` con una columna para cada categoría, necesitamos repetidor para emitir marcado similar al siguiente:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

Puesto que el `<td>Category X</td>` texto es la parte que se repite, esta descripción aparecerá en la s ItemTemplate de repetidor. El marcado que aparece antes de que - `<table><tr>` -se colocarán en el `HeaderTemplate` mientras el marcado final - `</tr></table>` -se colocará en el `FooterTemplate`. Para especificar esta configuración de plantilla, vaya a la parte de la página ASP.NET declarativa haciendo clic en el botón de origen en la esquina inferior izquierda y el tipo en la sintaxis siguiente:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

Repetidor emite el marcado según lo especificado por sus plantillas, nada más y nada menos preciso. Figura 17 muestra el resultado de s de repetidor cuando se ven a través de un explorador.


[![Una sola fila HTML &lt;tabla&gt; enumera cada categoría en una columna independiente](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**Figura 17**: una sola fila HTML `<table>` enumera cada categoría en una columna independiente ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Paso 6: Mejorar la apariencia del repetidor

Puesto que el repetidor emite con precisión el marcado especificado por sus plantillas, debe ser no sorprende que no hay propiedades relacionadas con el estilo de repetidor. Para modificar la apariencia del contenido generado por el repetidor, debemos agregar manualmente el contenido necesario de HTML o CSS directamente a las plantillas de repetidor s.

En nuestro ejemplo, permiten s tiene las columnas de la categoría alternar colores de fondo, al igual que con las filas alternas de DataList. Para lograr esto, debemos asignar el `RowStyle` clase CSS para cada elemento del repetidor y `AlternatingRowStyle` clase CSS para cada elemento del repetidor alterno a través de la `ItemTemplate` y `AlternatingItemTemplate` plantillas, de este modo:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

Permiten s también agregar una fila de encabezado a la salida con el texto de las categorías de producto. Puesto que no queremos t saber cuántas columnas nuestro resultante `<table>` estará compuesta de, es usar la forma más sencilla de generar una fila de encabezado que está garantizada que abarcan todas las columnas *dos* `<table>` s. La primera `<table>` contendrá dos filas de la fila de encabezado y una fila que contiene la fila segunda, `<table>` que tiene una columna para cada categoría en el sistema. Es decir, debe emitir el siguiente marcado:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

El siguiente `HeaderTemplate` y `FooterTemplate` como resultado en el formato deseado:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

La figura 18 muestra el repetidor una vez realizados estos cambios.


[![Las columnas de la categoría alternan la propiedad de Color de fondo e incluye una fila de encabezado](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**Figura 18**: la categoría columnas alternativo en Color de fondo e incluye una fila de encabezado ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))


## <a name="summary"></a>Resumen

Si bien el control GridView hace fácil mostrar, editar, eliminar, ordenar y paginar los datos, la apariencia es muy cuadros y cuadrícula. Para tener más control sobre el aspecto, es necesario volver a controles DataList o de repetidor. Ambos controles muestran un conjunto de registros mediante plantillas en lugar de BoundFields, CheckBoxFields y así sucesivamente.

El control DataList se representa como HTML `<table>` que, de forma predeterminada, muestra cada registro del origen de datos en una sola fila de tabla, igual que un control GridView con una sola TemplateField. Sin embargo, como se verá en un tutorial posterior, el control DataList permitir varios registros que se mostrarán por cada fila de la tabla. Repetidor, por otro lado, estrictamente emite el marcado especificado en sus plantillas; no agrega ningún marcado adicional y, por tanto, normalmente se usa para mostrar datos en los elementos HTML distinto de un `<table>` (como en una lista con viñetas).

Si bien la DataList y repetidor ofrecen más flexibilidad en su salida representada, que carecen de muchas de las características integradas que se encuentran en GridView. Como examinaremos en las próximas tutoriales, algunas de estas características se pueden conectar en sin demasiada esfuerzo, pero ¿tenga en cuenta que utilizando a DataList o repetidor en lugar de GridView limitar las características que puede utilizar sin necesidad de implementar las características usted mismo.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisores para este tutorial eran Yaakov Ellis, Liz Shulok, Randy Schmidt y Stacy Park. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](nested-data-web-controls-cs.md)
> [Siguiente](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
