---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Iteración #3: agregar una validación de formulario (C#) | Microsoft Docs'
author: microsoft
description: En la tercera iteración, se agrega una validación de formulario básico. Evitamos personas desde el envío de un formulario sin completar los campos obligatorios. También hemos validar emai...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 532a5849007a5e8301da7c2801b553b9148679c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828152"
---
<a name="iteration-3--add-form-validation-c"></a>Iteración #3: agregar una validación de formulario (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descargue el código](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> En la tercera iteración, se agrega una validación de formulario básico. Evitamos personas desde el envío de un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creación de una aplicación de MVC de ASP.NET de la administración de contactos (C#)
  

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa desde el principio para finalizar. La aplicación de Contact Manager permite almacenar información de contacto - nombres, números de teléfono y direcciones de correo electrónico: para obtener una lista de personas.

Creamos la aplicación a través de varias iteraciones. Con cada iteración, mejorar gradualmente la aplicación. Es el objetivo de este enfoque de varias iteraciones para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el Administrador de contactos de la manera más sencilla posible. Se agrega compatibilidad con las operaciones de base de datos básicos: creación, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación parezca interesante. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.

- Iteración #3: agregar validación de formulario. En la tercera iteración, se agrega una validación de formulario básico. Evitamos personas desde el envío de un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación tenga un acoplamiento. En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que sea más fácil de mantener y modificar la aplicación de Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y la inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinta, realizamos nuestra aplicación más fácil de mantener y modificar mediante la adición de las pruebas unitarias. Hemos simular nuestras clases de modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración #6: usar el desarrollo controlado por pruebas. En esta iteración sexta, se agregan nuevas funciones a nuestra aplicación escribir pruebas unitarias en primer lugar y escribir código en las pruebas unitarias. En esta iteración, se agrega grupos de contactos.

- Iteración #7: agregar funcionalidad Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad con Ajax.


## <a name="this-iteration"></a>Esta iteración

En esta segunda iteración de la aplicación de Contact Manager, se agrega una validación de formulario básico. Evitamos personas desde el envío de un contacto sin escribir valores para los campos obligatorios. También hemos validar números de teléfono y direcciones de correo electrónico (consulte la figura 1).


[![El cuadro de diálogo nuevo proyecto](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Figura 01**: un formulario con la validación ([haga clic aquí para ver imagen en tamaño completo](iteration-3-add-form-validation-cs/_static/image2.png))


En esta iteración, agregamos la lógica de validación directamente a las acciones del controlador. En general, esto no es la manera recomendada de agregar validación a una aplicación ASP.NET MVC. Un mejor enfoque consiste en colocar una lógica de validación s la aplicación en otro [capa de servicio](http://martinfowler.com/eaaCatalog/serviceLayer.html). En la siguiente iteración, se refactoriza la aplicación de Contact Manager para hacer más fácil de mantener la aplicación.

En esta iteración, para simplificar las cosas, se escribe todo el código de validación a mano. En lugar de escribir el código de validación nosotros mismos nos podríamos aprovechar un marco de validación. Por ejemplo, puede usar Microsoft Enterprise Library validación Application Block (VAB) para implementar la lógica de validación de la aplicación de ASP.NET MVC. Para obtener más información sobre el bloque de aplicaciones de la validación, consulte:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Agregar validación a la vista de creación

Permiten s empiece por agregar lógica de validación a la vista de creación. Afortunadamente, dado que se genera la vista de creación con Visual Studio, la vista de creación ya contiene toda la lógica de interfaz de usuario necesarios para mostrar mensajes de validación. La vista de creación se encuentra en el listado 1.

**Listado 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Observe la llamada al método auxiliar Html.ValidationSummary() que aparece justo encima del formulario HTML. Si hay mensajes de error de validación, este método muestra los mensajes de validación en una lista con viñetas.

Observe, además, las llamadas a Html.ValidationMessage() que aparecen junto a cada campo de formulario. La aplicación auxiliar de ValidationMessage() muestra un mensaje de error de validación individuales. En el caso de la lista 1, se muestra un asterisco cuando hay un error de validación.

Por último, la aplicación auxiliar Html.TextBox() automáticamente representa una clase de hoja de estilos en cascada cuando hay un error de validación asociado con la propiedad mostrada por la aplicación auxiliar. La aplicación auxiliar Html.TextBox() representa una clase denominada **error de validación de entrada**.

Cuando se crea una nueva aplicación MVC de ASP.NET, una hoja de estilos denominada Site.css se crea automáticamente en la carpeta de contenido. Esta hoja de estilos contiene las siguientes definiciones de clases CSS relacionadas con la apariencia de los mensajes de error de validación:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

La clase de error de validación de campo se utiliza para definir el estilo de la salida representada por la aplicación auxiliar de Html.ValidationMessage(). La clase de error de validación de entrada se utiliza para definir el estilo del cuadro de texto (entrada) representada por la aplicación auxiliar de Html.TextBox(). La clase de errores de resumen de validación se utiliza para definir el estilo de la lista sin ordenar representada por la aplicación auxiliar de Html.ValidationSummary().

> [!NOTE] 
> 
> Puede modificar las clases de hoja de estilo que se describe en esta sección para personalizar la apariencia de los mensajes de error de validación.


## <a name="adding-validation-logic-to-the-create-action"></a>Agregar lógica de validación para la creación de acción

En este momento, la vista de creación nunca muestra mensajes de error de validación porque no nos hemos escrito la lógica para generar los mensajes. Con el fin de mostrar mensajes de error de validación, deberá agregar los mensajes de error para ModelState.

> [!NOTE] 
> 
> El método UpdateModel() agrega mensajes de error para ModelState automáticamente cuando hay un error al asignar el valor de un campo de formulario a una propiedad. Por ejemplo, si se intenta asignar la cadena "apple" a una propiedad de fecha de nacimiento que acepta valores de fecha y hora, el método UpdateModel() agrega un error para ModelState.


El método Create() modificado en el listado 2 contiene una sección nueva que valida las propiedades de la clase Contact antes de que se inserta el nuevo contacto en la base de datos.

**Listado 2 - Controllers\ContactController.cs (crear con la validación)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

La sección de validación aplica cuatro reglas de validación distintos:

- La propiedad FirstName debe tener una longitud mayor que cero (y no puede contener solamente espacios)
- La propiedad LastName debe tener una longitud mayor que cero (y no puede contener solamente espacios)
- Si la propiedad de teléfono tiene un valor (tiene una longitud mayor que 0), a continuación, la propiedad de teléfono debe coincidir con una expresión regular.
- Si la propiedad de correo electrónico tiene un valor (tiene una longitud mayor que 0), a continuación, la propiedad de correo electrónico debe coincidir con una expresión regular.

Cuando se produce una infracción de regla de validación, se agrega un mensaje de error para ModelState con la Ayuda del método AddModelError(). Cuando se agrega un mensaje a ModelState, proporcione el nombre de una propiedad y el texto de un mensaje de error de validación. Este mensaje de error se muestra en la vista mediante los métodos auxiliares Html.ValidationSummary() y Html.ValidationMessage().

Después de que se ejecutan las reglas de validación, se comprueba la propiedad IsValid de ModelState. La propiedad IsValid devuelve false cuando se han agregado los mensajes de error de validación para ModelState. Si se produce un error de validación, se vuelve a mostrar el formulario de creación con los mensajes de error.

> [!NOTE] 
> 
> He recibido las expresiones regulares para validar la dirección de correo electrónico y número de teléfono desde el repositorio de expresión regular en [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Agregar lógica de validación a la acción de edición

La acción Edit() actualiza un contacto. La acción Edit() necesita realizar exactamente la misma validación que la acción Create(). En lugar de duplicar el mismo código de validación, debemos refactorizamos el controlador de contacto para que tanto la acción Create() como Edit() llama al mismo método de validación.

La clase de controlador de contacto modificada está contenida en el listado 3. Esta clase tiene un nuevo método ValidateContact() que se llama en el Create() y Edit() las acciones.

**Listado 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Resumen

En esta iteración, agregamos la validación de la forma más básica a nuestra aplicación de Contact Manager. Nuestra lógica de validación impide que los usuarios enviar un contacto nuevo o editar un contacto existente sin proporcionar valores para las propiedades FirstName y LastName. Además, los usuarios deben proporcionar las direcciones de correo electrónico y números de teléfono válido.

En esta iteración, agregamos la lógica de validación a nuestra aplicación póngase en contacto con el Administrador de la manera más sencilla posible. Sin embargo, mezclar la lógica de validación en nuestra lógica de controlador creará problemas por nosotros a largo plazo. Nuestra aplicación será más difícil de mantener y modificar con el tiempo.

En la siguiente iteración, se va a refactorizar nuestra lógica de validación y lógica de acceso de la base de datos fuera de nuestro controladores. Se podrá aprovechar varios principios de diseño de software para poder crear una aplicación de acoplamiento más flexible y más fácil de mantener.

> [!div class="step-by-step"]
> [Anterior](iteration-2-make-the-application-look-nice-cs.md)
> [Siguiente](iteration-4-make-the-application-loosely-coupled-cs.md)
