---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Realizar una validación Simple (VB) | Microsoft Docs
author: StephenWalther
description: Obtenga información sobre cómo realizar la validación en una aplicación ASP.NET MVC. En este tutorial, Stephen Walther presenta para el estado del modelo y la aplicación auxiliar de validación HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 26900229630b25affe21a5bb801ef0247711d26b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399466"
---
<a name="performing-simple-validation-vb"></a>Realizar una validación Simple (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Obtenga información sobre cómo realizar la validación en una aplicación ASP.NET MVC. En este tutorial, Stephen Walther presenta para el estado del modelo y las aplicaciones auxiliares de validación HTML.


El objetivo de este tutorial es explicar cómo se puede realizar la validación dentro de una aplicación ASP.NET MVC. Por ejemplo, obtendrá información sobre cómo evitar que alguien envíe un formulario que no contiene un valor para un campo obligatorio. Aprenda a usar el estado del modelo y las aplicaciones auxiliares HTML de validación.

## <a name="understanding-model-state"></a>Estado del modelo de descripción

Use el estado del modelo - o más concretamente, el diccionario de estado - para representar errores de validación. Por ejemplo, la acción Create() en el listado 1 valida las propiedades de una clase de producto antes de agregar la clase de producto a una base de datos.


No estoy recomendar que agregar la lógica de validación o la base de datos a un controlador. Un controlador debe contener únicamente la lógica relacionada con control de flujo de la aplicación. Nos quedamos con un acceso directo a simplificar las cosas.


**Listado 1 - Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

En el listado 1, se validan las propiedades UnitsInStock, descripción y nombre de la clase de producto. Si cualquiera de estas propiedades se producirá un error en una prueba de validación se agrega un error en el diccionario de Estados del modelo (representado por la propiedad ModelState de la clase de controlador).

Si hay errores en el estado del modelo, a continuación, la propiedad ModelState.IsValid devuelve false. En ese caso, se vuelve a mostrar el formulario HTML para crear un nuevo producto. En caso contrario, si no hay ningún error de validación, el nuevo producto se agrega a la base de datos.

## <a name="using-the-validation-helpers"></a>Uso de las aplicaciones auxiliares de validación

El marco ASP.NET MVC incluye dos aplicaciones auxiliares de validación: el Ayudante Html.ValidationMessage() y la aplicación auxiliar de Html.ValidationSummary(). Use estas dos aplicaciones auxiliares en una vista para mostrar mensajes de error de validación.

Las aplicaciones auxiliares de Html.ValidationMessage() y Html.ValidationSummary() se utilizan en las vistas Create y Edit que se generan automáticamente por el scaffolding de ASP.NET MVC. Siga estos pasos para generar la vista de creación:

1. Haga clic en la acción Create() del controlador de producto y seleccione la opción de menú **agregar vista** (consulte la figura 1).
2. En el **agregar vista** cuadro de diálogo, active la casilla etiquetada **crear una vista fuertemente tipada** (consulte la figura 2).
3. Desde el **Ver clase de datos** lista desplegable, seleccione la clase de producto.
4. Desde el **ver contenido** lista desplegable, seleccione crear.
5. Haga clic en el botón **Agregar**.


Asegúrese de que compilar la aplicación antes de agregar una vista. En caso contrario, no aparecerá la lista de clases en el **Ver clase de datos** lista desplegable.


[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Figura 01**: adición de una vista ([haga clic aquí para ver imagen en tamaño completo](performing-simple-validation-vb/_static/image2.png))


[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Figura 02**: creación de una vista fuertemente tipada ([haga clic aquí para ver imagen en tamaño completo](performing-simple-validation-vb/_static/image4.png))


Después de completar estos pasos, obtenga la vista de creación en el listado 2.

**Listado 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

En el listado 2, la aplicación auxiliar de Html.ValidationSummary() se llama inmediatamente encima del formulario HTML. Esta aplicación auxiliar se utiliza para mostrar una lista de mensajes de error de validación. La aplicación auxiliar Html.ValidationSummary() representa los errores en una lista con viñetas.

Se llama a la aplicación auxiliar de Html.ValidationMessage() junto a cada uno de los campos de formulario HTML. Esta aplicación auxiliar se utiliza para mostrar un mensaje de error junto a un campo de formulario. En el caso de listado 2, la aplicación auxiliar de Html.ValidationMessage() muestra un asterisco cuando se produce un error.

La página en la figura 3 muestra los mensajes de error representados por las aplicaciones auxiliares de validación cuando se envía el formulario con los campos que faltan y valores no válidos.


[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Figura 03**: crear la vista enviada con problemas ([haga clic aquí para ver imagen en tamaño completo](performing-simple-validation-vb/_static/image6.png))


Tenga en cuenta que el aspecto del HTML entrada campos también se modifican cuando hay un error de validación. Las representaciones de aplicación auxiliar Html.TextBox() un *clase = "error de validación de entrada"* atributo cuando hay un error de validación asociado con la propiedad representada por la aplicación auxiliar de Html.TextBox().

Hay tres clases de hoja de estilos en cascada se usan para controlar la apariencia de los errores de validación:

- entrada:-error de validación - se aplica a la &lt;entrada&gt; representada por Html.TextBox() auxiliar de etiqueta.
- campo--error de validación - se aplica a la &lt;abarcan&gt; etiqueta representada por la aplicación auxiliar de Html.ValidationMessage().
- Resumen de errores de validación - se aplica a la &lt;ul&gt; etiqueta representada por la aplicación auxiliar de Html.ValidationSumamry().

Puede modificar estas clases de hoja de estilos en cascada y, por lo tanto, modificar la apariencia de los errores de validación, modificando el archivo Site.css ubicado en la carpeta de contenido.

> [!NOTE] 
> 
> La clase HtmlHelper incluye propiedades estáticas de sólo lectura para recuperar los nombres de la validación de CSS en relación con las clases. Estas propiedades estáticas se denominan ValidationInputCssClassName, ValidationFieldCssClassName y ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding validación y la validación de Postbinding

Si envía el formulario HTML para la creación de un producto y escriba un valor no válido para el campo de precio y ningún valor para el campo UnitsInStock, obtendrá los mensajes de validación que se muestra en la figura 4. ¿De dónde proceden estos mensajes de error de validación?


[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Figura 04**: errores de validación de Prebinding ([haga clic aquí para ver imagen en tamaño completo](performing-simple-validation-vb/_static/image8.png))


Hay realmente dos tipos de mensajes de error de validación - los generados antes de que los campos del formulario HTML están enlazados a una clase y los generan después de los campos del formulario se enlazan a la clase. En otras palabras, hay prebinding errores de validación y postbinding errores de validación.

La acción Create() expuesta por el controlador de producto en el listado 1 acepta una instancia de la clase de producto. La firma del método Create tiene este aspecto:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Los valores de los campos de formulario HTML desde el formulario de creación se enlazan a la clase productToCreate por lo que se denomina un enlazador de modelos. El enlazador de modelos predeterminado agrega un mensaje de error para el estado del modelo automáticamente cuando un campo de formulario no puede enlazar a una propiedad de formulario.

El enlazador de modelos predeterminado no puede enlazar la cadena "apple" a la propiedad del precio de la clase de producto. No se puede asignar una cadena a una propiedad decimal. Por lo tanto, el enlazador de modelos agrega un error en el estado del modelo.

El enlazador de modelos predeterminado también no puede asignar el valor Nothing a una propiedad que no acepta el valor Nothing. En concreto, el enlazador de modelos no puede asignar el valor Nothing a la propiedad UnitsInStock. Una vez más, el enlazador de modelos renunciará y agrega un mensaje de error al estado del modelo.

Si desea personalizar la apariencia de estos prebinding mensajes de error, a continuación, deberá crear las cadenas de recursos para estos mensajes.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era describir los mecanismos básicos de la validación en el marco de MVC de ASP.NET. Ha aprendido a usar el estado del modelo y las aplicaciones auxiliares HTML de validación. También analizamos la distinción entre prebinding y postbinding validación. En otros tutoriales, trataremos diversas estrategias para mover el código fuera de los controladores de validación a las clases de modelo.

> [!div class="step-by-step"]
> [Anterior](displaying-a-table-of-database-data-vb.md)
> [Siguiente](validating-with-the-idataerrorinfo-interface-vb.md)
