---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Realizar una validación Simple (VB) | Documentos de Microsoft
author: StephenWalther
description: Obtenga información acerca de cómo realizar la validación en una aplicación ASP.NET MVC. En este tutorial, Stephen Walther presenta se usa para modelar el estado y la aplicación auxiliar validation HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: efb98d87106e332fffb158e5f382d57fea778957
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="performing-simple-validation-vb"></a>Realizar una validación Simple (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Obtenga información acerca de cómo realizar la validación en una aplicación ASP.NET MVC. En este tutorial, Stephen Walther presenta se usa para modelar el estado y las aplicaciones auxiliares HTML de validación.


El objetivo de este tutorial es explicar cómo puede realizar la validación dentro de una aplicación de ASP.NET MVC. Por ejemplo, aprenderá cómo impedir que alguien enviar un formulario que no contiene un valor para un campo obligatorio. Obtenga información acerca de cómo usar el estado del modelo y las aplicaciones auxiliares HTML de validación.

## <a name="understanding-model-state"></a>Estado del modelo de descripción

Usar estado del modelo - o más concretamente, el diccionario de Estados del modelo - para representar errores de validación. Por ejemplo, la acción Create() en el listado 1 valida las propiedades de una clase de producto antes de agregar la clase de producto a una base de datos.


No estoy recomendación de que se agrega la lógica de validación o la base de datos a un controlador. Un controlador debe contener solo lógica relacionada con el control de flujo de aplicación. Nos quedamos con un acceso directo para no complicar las cosas.


**Lista 1 - Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

En la lista 1, se validan las propiedades nombre, descripción y UnitsInStock de la clase de producto. Si cualquiera de estas propiedades producirá un error en una prueba de validación se agrega un error en el diccionario de Estados del modelo (representado por la propiedad ModelState de la clase de controlador).

Si hay errores en el estado del modelo, a continuación, la propiedad ModelState.IsValid devuelve false. En ese caso, se vuelve a mostrar en el formulario HTML para crear un nuevo producto. En caso contrario, si no hay ningún error de validación, el nuevo producto se agrega a la base de datos.

## <a name="using-the-validation-helpers"></a>Uso de las aplicaciones auxiliares de validación

El marco de ASP.NET MVC incluye dos aplicaciones auxiliares de validación: la aplicación auxiliar Html.ValidationMessage() y la aplicación auxiliar Html.ValidationSummary(). Usar estas aplicaciones auxiliares de dos en una vista para mostrar mensajes de error de validación.

Las aplicaciones auxiliares Html.ValidationMessage() y Html.ValidationSummary() se utilizan en las vistas de creación y edición que se generan automáticamente con el scaffolding de ASP.NET MVC. Siga estos pasos para generar la vista de creación:

1. Haga clic en la acción Create() en el controlador de producto y seleccione la opción de menú **agregar vista** (consulte la figura 1).
2. En el **agregar vista** cuadro de diálogo, active la casilla etiquetada **crear una vista fuertemente tipada** (consulte la figura 2).
3. Desde el **ver datos clase** lista desplegable, seleccione la clase de producto.
4. Desde el **ver contenido** lista desplegable, seleccione crear.
5. Haga clic en el botón **Agregar**.


Asegúrese de que compila la aplicación antes de agregar una vista. En caso contrario, no aparecerá la lista de clases en el **ver datos clase** lista desplegable.


[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Figura 01**: agregar una vista ([haga clic aquí para ver la imagen a tamaño completo](performing-simple-validation-vb/_static/image2.png))


[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Figura 02**: crear una vista fuertemente tipada ([haga clic aquí para ver la imagen a tamaño completo](performing-simple-validation-vb/_static/image4.png))


Después de completar estos pasos, obtener la vista de creación en el listado 2.

**La lista 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

En el listado 2, la aplicación auxiliar Html.ValidationSummary() se llama inmediatamente encima del formulario HTML. Esta aplicación auxiliar se utiliza para mostrar una lista de mensajes de error de validación. La aplicación auxiliar Html.ValidationSummary() representa los errores en una lista con viñetas.

Se llama a la aplicación auxiliar Html.ValidationMessage() junto a cada uno de los campos de formulario HTML. Esta aplicación auxiliar se utiliza para mostrar un mensaje de error junto a un campo de formulario. En el caso de listado 2, la aplicación auxiliar Html.ValidationMessage() muestra un asterisco cuando se produce un error.

La página en la figura 3 muestra los mensajes de error que se representan por las aplicaciones auxiliares de validación cuando se envía el formulario con los campos que faltan y valores no válidos.


[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Figura 03**: crear la vista se ha enviado con problemas ([haga clic aquí para ver la imagen a tamaño completo](performing-simple-validation-vb/_static/image6.png))


Tenga en cuenta que también se modifican los campos cuando se produce un error de validación de entrada de la apariencia del HTML. La aplicación auxiliar Html.TextBox() se representan una *clase = "error de validación de entrada"* atributo cuando se produce un error de validación asociado con la propiedad representada por la aplicación auxiliar Html.TextBox().

Hay tres clases de hoja de estilos en cascada permiten controlar la apariencia de los errores de validación:

- error de entrada-validación - se aplica a la &lt;entrada&gt; etiqueta representada por la aplicación auxiliar de Html.TextBox().
- campo--error de validación - se aplica a la &lt;abarcan&gt; etiqueta representada por la aplicación auxiliar Html.ValidationMessage().
- Resumen-errores de validación: se aplica a la &lt;ul&gt; etiqueta representada por la aplicación auxiliar Html.ValidationSumamry().

Puede modificar estas clases de hoja de estilos en cascada y, por lo tanto, modificar el aspecto de los errores de validación, modificando el archivo Site.css ubicado en la carpeta de contenido.

> [!NOTE] 
> 
> La clase HtmlHelper incluye propiedades estáticas de solo lectura para recuperar los nombres de la validación relacionados con CSS clases. Estas propiedades estáticas se denominan ValidationInputCssClassName, ValidationFieldCssClassName y ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding validación y la validación de Postbinding

Si se envía el formulario HTML para la creación de un producto y escriba un valor no válido para el campo de precio y ningún valor para el campo UnitsInStock, obtendrá los mensajes de validación que se muestra en la figura 4. ¿De dónde proceden estos mensajes de error de validación?


[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Figura 04**: errores de validación de Prebinding ([haga clic aquí para ver la imagen a tamaño completo](performing-simple-validation-vb/_static/image8.png))


Existen realmente dos tipos de mensajes de error de validación - los generados antes de que los campos de formulario HTML están enlazados a una clase y las que generan después de los campos del formulario se enlazan a la clase. En otras palabras, no hay errores de validación de prebinding y postbinding errores de validación.

La acción de Create() expuesta por el controlador de producto en la lista 1 acepta una instancia de la clase de producto. La firma del método Create tiene el siguiente aspecto:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Los valores de los campos de formulario HTML desde el formulario de creación se enlazan a la clase productToCreate por lo que se denomina un enlazador de modelos. El enlazador de modelos predeterminado agrega un mensaje de error al estado modelo automáticamente cuando un campo de formulario no puede enlazar a una propiedad de formulario.

El enlazador de modelos predeterminado no puede enlazar la cadena "apple" para la propiedad de precio de la clase de producto. No se puede asignar una cadena a una propiedad decimal. Por lo tanto, el enlazador de modelos agrega un error al estado del modelo.

El enlazador de modelos predeterminado también no puede asignar el valor Nothing a una propiedad que no acepta el valor Nothing. En concreto, el enlazador de modelos no puede asignar el valor Nothing a la propiedad UnitsInStock. Una vez más, el enlazador de modelos desiste y agrega un mensaje de error al estado del modelo.

Si desea personalizar la apariencia de estos prebinding mensajes de error, a continuación, debe crear cadenas de recursos para estos mensajes.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era describir los mecanismos básicos de validación en el marco de MVC de ASP.NET. Aprendió a utilizar el estado del modelo y las aplicaciones auxiliares HTML de validación. También se explica la distinción entre prebinding y postbinding validación. En otros tutoriales, analizaremos diversas estrategias para mover el código de validación de los controladores y en las clases de modelo.

> [!div class="step-by-step"]
> [Anterior](displaying-a-table-of-database-data-vb.md)
> [Siguiente](validating-with-the-idataerrorinfo-interface-vb.md)
