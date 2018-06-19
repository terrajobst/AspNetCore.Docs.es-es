---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iteración #3: agregar validación del formulario (VB) | Documentos de Microsoft'
author: microsoft
description: En la tercera iteración, agregamos la validación del formulario básico. Es evitar que los usuarios enviar un formulario sin completar los campos obligatorios. También se valida emai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e30e247bd31dfb800eea517d195025f9e881cd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870445"
---
<a name="iteration-3--add-form-validation-vb"></a>Iteración #3: agregar validación del formulario (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> En la tercera iteración, agregamos la validación del formulario básico. Es evitar que los usuarios enviar un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de MVC de ASP.NET (VB) de administración de contactos
  

En esta serie de tutoriales, creamos una aplicación de administración de contactos toda desde el principio para finalizar. La aplicación de ponerse en contacto con el administrador permite almacenar información de contacto - nombres, números de teléfono y direcciones de correo electrónico: para obtener una lista de personas.

Se compile la aplicación en varias iteraciones. Con cada iteración, se mejora gradualmente la aplicación. El objetivo de este enfoque de iteración varias es para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el póngase en contacto con el Administrador de la manera más sencilla posible. Se agrega compatibilidad para las operaciones de base de datos básicos: crear, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación sea bonito. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de vista de MVC de ASP.NET predeterminada y en cascada hoja de estilos.

- Iteración #3: agregar validación del formulario. En la tercera iteración, agregamos la validación del formulario básico. Es evitar que los usuarios enviar un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación de acoplamiento flexible. En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que resulten más fáciles de mantener y modificar la aplicación póngase en contacto con el administrador. Por ejemplo, se refactoriza nuestra aplicación para que use el modelo de repositorio y el modelo de inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinto, hacemos nuestra aplicación más fácil de mantener y modificar mediante la adición de pruebas unitarias. Hemos simular nuestro clases del modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración 6 #: Use desarrollo controlado por pruebas. En esta iteración sexto, se agregan nuevas funciones a nuestra aplicación escribiendo pruebas unitarias en primer lugar y escribir código frente a las pruebas unitarias. En esta iteración, agregamos grupos de contactos.

- Iteración #7 - agregar funcionalidad de Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad para Ajax.


## <a name="this-iteration"></a>Esta iteración

En esta segunda iteración de la aplicación Administrador de contacto, agregamos la validación del formulario básico. Es evitar que los usuarios envíen un contacto sin introducir valores para los campos obligatorios. También hemos validar números de teléfono y direcciones de correo electrónico (consulte la figura 1).


[![El cuadro de diálogo nuevo proyecto](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Figura 01**: un formulario con la validación ([haga clic aquí para ver la imagen a tamaño completo](iteration-3-add-form-validation-vb/_static/image2.png))


En esta iteración, agregamos la lógica de validación directamente a las acciones de controlador. En general, esto no es la manera recomendada de agregar validación a una aplicación ASP.NET MVC. Un enfoque más adecuado consiste en colocar una lógica de validación de s de aplicación en otro [capa de servicio](http://martinfowler.com/eaaCatalog/serviceLayer.html). En la siguiente iteración, se refactoriza la aplicación póngase en contacto con el administrador para que la aplicación sea más fácil de mantener.

En esta iteración, para que sea más sencillo, escribimos todo el código de validación a mano. En lugar de escribir el código de validación nosotros mismos, se puede sacar partido de un marco de validación. Por ejemplo, puede utilizar Microsoft Enterprise Library validación aplicación bloque (VAB) para implementar la lógica de validación para la aplicación de ASP.NET MVC. Para obtener más información sobre el bloqueo de aplicación de validación, vea:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Agregar validación a la vista de creación

Permiten s empiece por agregar lógica de validación a la vista de creación. Afortunadamente, dado que se genera la vista de creación con Visual Studio, la vista de creación ya contiene toda la lógica de la interfaz de usuario necesarios para mostrar mensajes de validación. La vista de creación se encuentra en la lista 1.

**Lista 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

La clase de error de validación de campo se utiliza para definir el estilo de la salida representada por la aplicación auxiliar Html.ValidationMessage(). La clase de error de validación de entrada se utiliza para definir el estilo del cuadro de texto (entrada) representada por la aplicación auxiliar Html.TextBox(). La clase de errores de resumen de validación se utiliza para definir el estilo de la lista sin ordenar representada por la aplicación auxiliar Html.ValidationSummary().

> [!NOTE] 
> 
> Puede modificar las clases de hoja de estilo se describen en esta sección para personalizar la apariencia de los mensajes de error de validación.


## <a name="adding-validation-logic-to-the-create-action"></a>Agregar lógica de validación a la acción de crear

En este momento, la vista de creación nunca muestra mensajes de error de validación porque no lo hemos incluido la lógica para generar los mensajes. Para mostrar mensajes de error de validación, debe agregar los mensajes de error para ModelState.

> [!NOTE] 
> 
> El método UpdateModel() agrega mensajes de error para ModelState automáticamente cuando hay un error al asignar el valor de un campo de formulario a una propiedad. Por ejemplo, si intenta asignar la cadena "apple" a una propiedad de fecha de nacimiento que acepta valores de fecha y hora, el método UpdateModel() agrega un error a ModelState.


El método Create() modificado en el listado 2 contiene una sección nueva que valida las propiedades de la clase Contact antes de que el nuevo contacto se inserta en la base de datos.

**La lista 2 - Controllers\ContactController.vb (crear con la validación)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

La sección Validar exige cuatro reglas de validación distintos:

- La propiedad FirstName debe tener una longitud mayor que cero (y no puede constar únicamente de espacios)
- La propiedad LastName debe tener una longitud mayor que cero (y no puede constar únicamente de espacios)
- Si la propiedad de teléfono tiene un valor (tiene una longitud mayor que 0), a continuación, la propiedad de teléfono debe coincidir con una expresión regular.
- Si la propiedad de correo electrónico tiene un valor (tiene una longitud mayor que 0), a continuación, la propiedad de correo electrónico debe coincidir con una expresión regular.

Cuando se produce una infracción de regla de validación, se agrega un mensaje de error para ModelState con la Ayuda del método AddModelError(). Cuando se agrega un mensaje a ModelState, proporcione el nombre de una propiedad y el texto de un mensaje de error de validación. Este mensaje de error se muestra en la vista mediante los métodos de aplicación auxiliar Html.ValidationSummary() y Html.ValidationMessage().

Una vez que se ejecutan las reglas de validación, se comprueba la propiedad IsValid de ModelState. La propiedad IsValid devuelve false cuando los mensajes de error de validación se han agregado a ModelState. Si se produce un error en la validación, se vuelve a mostrar el formulario de creación con los mensajes de error.

> [!NOTE] 
> 
> Tengo que las expresiones regulares para validar la dirección de correo electrónico y número de teléfono desde el repositorio de expresión regular en [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Agregar lógica de validación a la acción de modificación

La acción Edit() actualiza un contacto. La acción Edit() necesita realizar exactamente la misma validación que la acción Create(). En lugar de duplicar el mismo código de validación, deberíamos refactorizamos el controlador de contacto para que el método Create() y Edit() acciones llamar al mismo método de validación.

La clase de controlador de contacto modificada se encuentra en el listado 3. Esta clase tiene un nuevo método ValidateContact() que se llama en el método Create() y Edit() acciones.

**El listado 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Resumen

En esta iteración, agregamos la validación del formulario básico a nuestra aplicación póngase en contacto con el administrador. Nuestra lógica de validación impide que los usuarios enviar un contacto nuevo o editar un contacto ya existente sin proporcionar valores para las propiedades FirstName y LastName. Además, los usuarios deben proporcionar direcciones de correo electrónico y de números de teléfono válido.

En esta iteración, agregamos la lógica de validación a nuestra aplicación póngase en contacto con el Administrador de la manera más sencilla posible. Sin embargo, la combinación de nuestra lógica de validación en nuestra lógica de controlador causará problemas para que podamos a largo plazo. Nuestra aplicación será más difícil de mantener y modificar con el tiempo.

En la siguiente iteración, se va a refactorizar nuestra lógica de validación y la lógica de acceso de la base de datos fuera de nuestro controladores. Se podrá sacar partido de varios principios de diseño de software para poder crear una aplicación de acoplamiento más flexible y más fácil de mantener.

> [!div class="step-by-step"]
> [Anterior](iteration-2-make-the-application-look-nice-vb.md)
> [Siguiente](iteration-4-make-the-application-loosely-coupled-vb.md)
