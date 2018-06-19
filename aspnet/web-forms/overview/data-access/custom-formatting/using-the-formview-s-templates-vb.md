---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Mediante las plantillas de FormView (VB) | Documentos de Microsoft
author: rick-anderson
description: A diferencia de DetailsView, FormView no se compone de campos. En su lugar, FormView se representa mediante plantillas. En este tutorial examinaremos mediante la F....
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 16293960f5d8758c93646844bd159547f5e0f38c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875154"
---
<a name="using-the-formviews-templates-vb"></a>Uso de las plantillas de FormView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) o [descarga de PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> A diferencia de DetailsView, FormView no se compone de campos. En su lugar, FormView se representa mediante plantillas. En este tutorial que examinaremos usando el control FormView para presentar una presentación menos rígida de datos.


## <a name="introduction"></a>Introducción

En los dos últimos tutoriales, hemos visto cómo personalizar las salidas de los controles GridView y DetailsView mediante TemplateFields. TemplateFields permitir para el contenido de un campo específico para personalizarse alta, pero al final GridView y DetailsView tienen un aspecto cuadros en su lugar, la cuadrícula. Para muchos escenarios tal un diseño de cuadrícula es ideal, pero en ocasiones se necesita una presentación más fluida, menos rígida. Cuando se muestra un único registro, un diseño fluido de este tipo es posible mediante el control FormView.

A diferencia de DetailsView, FormView no se compone de campos. No se puede agregar un BoundField o TemplateField a un FormView. En su lugar, FormView se representa mediante plantillas. Considerar la FormView como un control de DetailsView que contiene un único TemplateField. FormView admite las siguientes plantillas:

- `ItemTemplate` usa para representar el registro concreto que se muestran en FormView
- `HeaderTemplate` se utiliza para especificar una fila de encabezado opcional
- `FooterTemplate` se utiliza para especificar una fila de pie de página opcional
- `EmptyDataTemplate` Cuando el FormView `DataSource` no tiene ningún registro, el `EmptyDataTemplate` se utiliza en lugar de la `ItemTemplate` para representar el marcado del control
- `PagerTemplate` puede usarse para personalizar la interfaz de paginación para FormViews que tienen habilitada la paginación
- `EditItemTemplate` / `InsertItemTemplate` usar para personalizar la interfaz de edición o la interfaz de inserción para FormViews que admiten esta funcionalidad

En este tutorial que examinaremos usando el control FormView para presentar una presentación menos rígida de productos. En lugar de tener campos para el nombre, categoría, proveedor y así sucesivamente, FormView `ItemTemplate` mostrarán estos valores mediante una combinación de un elemento de encabezado y un `<table>` (consulte la figura 1).


[![FormView se interrumpe del diseño de cuadrícula aparece en DetailsView](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Figura 1**: FormView saltos fuera de la muestra de diseño de Grid-Like en DetailsView ([haga clic aquí para ver la imagen a tamaño completo](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Paso 1: Enlace de datos a FormView

Abra la `FormView.aspx` página y arrastre un FormView del cuadro de herramientas hasta el diseñador. Cuando se agrega primero el FormView aparece como un cuadro gris, que nos indica que un `ItemTemplate` es necesario.


[![FormView no se puede representar en el diseñador hasta que se proporcione un ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Figura 2**: The FormView no puede representar en el diseñador hasta que un `ItemTemplate` se proporciona ([haga clic aquí para ver la imagen a tamaño completo](using-the-formview-s-templates-vb/_static/image6.png))


La `ItemTemplate` puede crearse manualmente (a través de la sintaxis declarativa) o puede ser creadas automáticamente mediante el enlace FormView a un control de origen de datos a través del diseñador. Esto crea automáticamente `ItemTemplate` contiene HTML que enumera el nombre de cada campo y una etiqueta de control cuya `Text` propiedad se enlaza con el valor del campo. Este enfoque también auto-crea una `InsertItemTemplate` y `EditItemTemplate`, ambos de los cuales se rellenan con los controles de entrada para cada uno de los campos de datos devueltos por el control de origen de datos.

Si desea crear automáticamente la plantilla, de la etiqueta inteligente de FormView agregar un nuevo control ObjectDataSource que invoca la `ProductsBLL` la clase `GetProducts()` método. Esto creará un FormView con un `ItemTemplate`, `InsertItemTemplate`, y `EditItemTemplate`. En la vista del origen, quite el `InsertItemTemplate` y `EditItemTemplate` ya que no estamos interesados en crear un FormView que admita modificar o insertar todavía. Después, desactive espera el marcado en el `ItemTemplate` para que tengamos una pizarra limpia para trabajar desde.

Si generará en su lugar el `ItemTemplate` manualmente, puede agregar y configurar el ObjectDataSource arrastrándolo desde el cuadro de herramientas hasta el diseñador. Sin embargo, no se establece el origen de datos de FormView desde el diseñador. En su lugar, vaya a la vista del origen y establecer manualmente la FormView `DataSourceID` propiedad a la `ID` valor de ObjectDataSource. A continuación, agregar manualmente el `ItemTemplate`.

Independientemente de qué enfoque decidió que llevar a cabo en este momento marcado declarativo de la FormView debe apariencia:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Tómese un momento para que se active la casilla Habilitar paginación en una etiqueta inteligente de FormView; Esto agregará la `AllowPaging="True"` atribuir a la sintaxis declarativa de FormView. Además, establezca el `EnableViewState` propiedad en False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Paso 2: Definir la`ItemTemplate`del marcado

Con el FormView enlazado al control ObjectDataSource y configurado para admitir la paginación estamos listos especificar el contenido de la `ItemTemplate`. Para este tutorial, supongamos que tiene el nombre del producto mostrado en un `<h3>` encabezado. A continuación, vamos a usar HTML `<table>` para mostrar las propiedades restantes del producto en una tabla de cuatro columnas donde las columnas primeros y terceros enumeran los nombres de propiedad y la segunda y la cuarta lista de sus valores.

Este marcado puede especificarse en a través de la interfaz de edición de plantilla de FormView en el diseñador o escribir manualmente mediante la sintaxis declarativa. Trabajar con plantillas de normalmente resulta más rápido trabajar directamente con la sintaxis declarativa, pero no dude en usar independientemente de la técnica se sienta más cómodo con.

El marcado siguiente muestra el marcado declarativo FormView después de la `ItemTemplate`de estructura se ha completado:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Tenga en cuenta que la sintaxis de enlace de datos - `<%# Eval("ProductName") %>`, por ejemplo la puede insertar directamente en la salida de la plantilla. Es decir, que necesita no asignarse a un control de etiqueta `Text` propiedad. Por ejemplo, tenemos la `ProductName` valor mostrado en un `<h3>` elemento mediante `<h3><%# Eval("ProductName") %></h3>`, que para el producto Chai se representará como `<h3>Chai</h3>`.

El `ProductPropertyLabel` y `ProductPropertyValue` clases CSS que se usan para especificar el estilo de los nombres de propiedad de producto y valores en el `<table>`. Estas clases CSS se definen en `Styles.css` y hacer que los nombres de propiedad a negrita y alineado a la derecha y agregue un relleno para los valores de propiedad del lado derecho.

Puesto que no hay ninguna CheckBoxFields disponible con FormView, con el fin de mostrar la `Discontinued` valor como una casilla de verificación debemos agregar nuestro propio control de casilla de verificación. El `Enabled` propiedad está establecida en False, lo que sea de sólo lectura y la casilla de verificación `Checked` propiedad se enlaza con el valor de la `Discontinued` campo de datos.

Con el `ItemTemplate` haya finalizado, se muestra la información de producto de una manera mucho más fluida. Compare el resultado de DetailsView desde el último tutorial (ilustración 3) con la salida generada por el FormView en este tutorial (ilustración 4).


[![La salida de DetailsView rígida](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Figura 3**: la salida de DetailsView rígida ([haga clic aquí para ver la imagen a tamaño completo](using-the-formview-s-templates-vb/_static/image9.png))


[![La salida de FormView fluido](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Figura 4**: la salida de FormView fluido ([haga clic aquí para ver la imagen a tamaño completo](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>Resumen

Mientras que los controles GridView y DetailsView pueden tener su resultado personalizado mediante TemplateFields, todavía ambos presentan sus datos en un formato de cuadrícula, cuadros. En esas ocasiones cuando debe mostrar un único registro con un diseño menos rígido, FormView es una opción ideal. Al igual que el DetailsView FormView representa un único registro de su `DataSource`, pero a diferencia de DetailsView se compone solo de plantillas y no admite campos.

Como hemos visto en este tutorial, FormView permite un diseño más flexible al mostrar un único registro. En tutoriales futuros examinaremos los controles DataList y repetidor, que proporcionan el mismo nivel de flexibilidad como el FormsView, pero pueden incluir varios registros (como GridView).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor principal para este tutorial estaba E.R. Gil. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-detailsview-control-vb.md)
> [Siguiente](displaying-summary-information-in-the-gridview-s-footer-vb.md)
