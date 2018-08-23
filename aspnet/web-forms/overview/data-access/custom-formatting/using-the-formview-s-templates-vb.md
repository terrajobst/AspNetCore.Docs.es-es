---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Uso de las plantillas de FormView (VB) | Microsoft Docs
author: rick-anderson
description: A diferencia de DetailsView, FormView no se compone de campos. En su lugar, FormView se representa mediante plantillas. En este tutorial examinaremos utilizando la F...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 6d16ef7ef8a3d5fce10e0d0b88421be294e9fc8d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835396"
---
<a name="using-the-formviews-templates-vb"></a>Uso de las plantillas de FormView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) o [descargar PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> A diferencia de DetailsView, FormView no se compone de campos. En su lugar, FormView se representa mediante plantillas. En este tutorial que se examinarán utilizando el control FormView para presentar una presentación de datos menos rígida.


## <a name="introduction"></a>Introducción

En los dos últimos tutoriales, vimos cómo personalizar el uso de TemplateFields de salidas de los controles GridView y DetailsView. Permitir el contenido de un campo específico para personalizarse altamente TemplateFields, pero al final GridView y DetailsView tienen un aspecto similar a cuadrícula anguloso en su lugar. Para muchos escenarios tal un diseño de cuadrícula es ideal, pero en ocasiones se necesita una presentación más fluida y menos rígida. Cuando se muestra un único registro, es posible mediante el control FormView tal un diseño fluido.

A diferencia de DetailsView, FormView no se compone de campos. No se puede agregar un BoundField o TemplateField en un FormView. En su lugar, FormView se representa mediante plantillas. Considerar un control DetailsView que contiene un único TemplateField de FormView. FormView admite las siguientes plantillas:

- `ItemTemplate` utilizado para representar el registro concreto que se muestra en FormView
- `HeaderTemplate` se utiliza para especificar una fila de encabezado opcional
- `FooterTemplate` se utiliza para especificar una fila de pie de página opcional
- `EmptyDataTemplate` Cuando el FormView `DataSource` carece de los registros, el `EmptyDataTemplate` se utiliza en lugar de la `ItemTemplate` para representar el marcado del control
- `PagerTemplate` puede usarse para personalizar la interfaz de paginación para FormViews que tienen habilitada la paginación
- `EditItemTemplate` / `InsertItemTemplate` usar para personalizar la interfaz de edición o la interfaz de inserción para FormViews que admiten esta funcionalidad

En este tutorial que se examinarán utilizando el control FormView para presentar una presentación menos rígida de productos. En lugar de tener campos para el nombre, categoría, proveedores y así sucesivamente, FormView `ItemTemplate` mostrará estos valores mediante una combinación de un elemento de encabezado y un `<table>` (consulte la figura 1).


[![FormView se interrumpe del diseño de cuadrícula visto en DetailsView](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Figura 1**: FormView sale el diseño Grid-Like visto en DetailsView ([haga clic aquí para ver imagen en tamaño completo](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Paso 1: Enlace de datos a FormView

Abra el `FormView.aspx` página y arrastre un FormView desde el cuadro de herramientas hasta el diseñador. Cuando se agrega primero FormView aparece como un cuadro gris, que nos indica que un `ItemTemplate` es necesaria.


[![FormView no se puede representar en el diseñador hasta que se proporcione una plantilla ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Figura 2**: The FormView no se puede representar en el Diseñador de hasta un `ItemTemplate` se proporciona ([haga clic aquí para ver imagen en tamaño completo](using-the-formview-s-templates-vb/_static/image6.png))


El `ItemTemplate` pueden crearse de forma manual (a través de la sintaxis declarativa) o puede ser creado automáticamente mediante el enlace a un control de origen de datos a través del Diseñador de FormView. Esto crea automáticamente `ItemTemplate` contiene código HTML que enumera el nombre de cada campo y una etiqueta de control cuya `Text` está enlazada la propiedad para el valor del campo. Este enfoque también auto-crea una `InsertItemTemplate` y `EditItemTemplate`, ambos de los cuales se rellenan con los controles de entrada para cada uno de los campos de datos devueltos por el control de origen de datos.

Si desea crear automáticamente la plantilla, de la etiqueta inteligente de FormView agregar un nuevo control ObjectDataSource que invoca la `ProductsBLL` la clase `GetProducts()` método. Esto creará un FormView con un `ItemTemplate`, `InsertItemTemplate`, y `EditItemTemplate`. En la vista del origen, quite el `InsertItemTemplate` y `EditItemTemplate` como no estamos interesados en crear un FormView que admite edición o insertar todavía. A continuación, limpiar el marcado dentro de la `ItemTemplate` para que tengamos una pizarra limpia para que funcione desde.

Si prefiere crear el `ItemTemplate` manualmente, puede agregar y configurar el origen ObjectDataSource arrastrándolo desde el cuadro de herramientas hasta el diseñador. Sin embargo, no establezca origen de datos de FormView desde el diseñador. En su lugar, vaya a la vista del origen y configurar manualmente el FormView `DataSourceID` propiedad a la `ID` valor del origen ObjectDataSource. A continuación, agregue manualmente el `ItemTemplate`.

Independientemente de qué enfoque ha decidido aprovechar, en este punto marcado declarativo de su FormView aspecto que deberían:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Dedique unos minutos a que se active la casilla Habilitar paginación en la etiqueta inteligente de FormView; Esto agregará la `AllowPaging="True"` atributo a la sintaxis declarativa de FormView. Además, establezca el `EnableViewState` propiedad en False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Paso 2: Definir la`ItemTemplate`del marcado

Con FormView enlazado al control ObjectDataSource y configurado para admitir la paginación ya estamos listos para especificar el contenido de la `ItemTemplate`. Para este tutorial, vamos a tener el nombre del producto aparece en un `<h3>` encabezado. A continuación, vamos a usar HTML `<table>` para mostrar las propiedades restantes del producto en una tabla de cuatro columnas, donde las columnas primeros y terceros lista de los nombres de propiedad y la segunda y la cuarta lista de sus valores.

Este marcado se puede escribir en a través de la interfaz de edición de plantillas de FormView en el diseñador o escribir manualmente mediante la sintaxis declarativa. Cuando se trabaja con plantillas normalmente le resulta más rápida al trabajar directamente con la sintaxis declarativa, pero no dude en usar independientemente de la técnica se sienten más cómodos.

El marcado siguiente muestra el marcado declarativo FormView después el `ItemTemplate`de estructura se ha completado:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Tenga en cuenta que la sintaxis de enlace de datos - `<%# Eval("ProductName") %>`, por ejemplo puede insertar directamente en la salida de la plantilla. Es decir, se necesita no asignar a un control de etiqueta `Text` propiedad. Por ejemplo, tenemos la `ProductName` valor mostrado en un `<h3>` elemento mediante `<h3><%# Eval("ProductName") %></h3>`, lo que, para el producto Chai se representará como `<h3>Chai</h3>`.

El `ProductPropertyLabel` y `ProductPropertyValue` clases CSS se usan para especificar el estilo de los nombres de propiedad de producto y los valores de la `<table>`. Estas clases CSS se definen en `Styles.css` y hacer que los nombres de propiedad en negrita y alineado a la derecha y agregar un derecho de relleno para los valores de propiedad.

Puesto que no hay ningún CheckBoxFields disponibles con FormView, con el fin de mostrar el `Discontinued` valor como una casilla de verificación que debemos agregar nuestro propio control de casilla de verificación. El `Enabled` propiedad está establecida en False, lo que sea de solo lectura y la casilla de verificación `Checked` propiedad se enlaza al valor de la `Discontinued` campo de datos.

Con el `ItemTemplate` haya finalizado, se muestra la información de producto de una manera mucho más fluida. Compare el resultado de DetailsView desde el último tutorial (figura 3) con la salida generada por FormView en este tutorial (figura 4).


[![La salida de DetailsView rígido](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Figura 3**: la salida DetailsView rígido ([haga clic aquí para ver imagen en tamaño completo](using-the-formview-s-templates-vb/_static/image9.png))


[![La salida de FormView fluido](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Figura 4**: la salida de FormView dinámica de fluidos ([haga clic aquí para ver imagen en tamaño completo](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>Resumen

Mientras que los controles GridView y DetailsView pueden tener su resultado personalizado de uso de TemplateFields, ambos sigue presentan sus datos en un formato de cuadrícula y anguloso. Para aquellas ocasiones cuando debe mostrar un único registro con un diseño menos rígido, FormView es una opción ideal. Al igual que DetailsView, FormView representa un único registro de su `DataSource`, pero a diferencia de DetailsView se compone simplemente de plantillas y no admite campos.

Como hemos visto en este tutorial, FormView permite un diseño más flexible al mostrar un único registro. En tutoriales futuros se examinarán los controles DataList y Repeater, que proporcionan el mismo nivel de flexibilidad como el FormsView, pero se pueden mostrar varios registros (por ejemplo, el control GridView).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor principal para este tutorial era E.R. Gil. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-detailsview-control-vb.md)
> [Siguiente](displaying-summary-information-in-the-gridview-s-footer-vb.md)
