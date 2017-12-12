---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: "Iteración #2: hacer que la aplicación buscar nice (C#) | Documentos de Microsoft"
author: microsoft
description: "En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de vista de MVC de ASP.NET predeterminada y en cascada hoja de estilos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 10379f5321773155aaff4c384d8e0716d7e0e874
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="iteration-2--make-the-application-look-nice-c"></a>Iteración #2: hacer que la aplicación buscar nice (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de vista de MVC de ASP.NET predeterminada y en cascada hoja de estilos.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creación de una aplicación de MVC de ASP.NET de administración de contactos (C#)
  

En esta serie de tutoriales, creamos una aplicación de administración de contactos toda desde el principio para finalizar. La aplicación de ponerse en contacto con el administrador permite almacenar información de contacto - nombres, números de teléfono y direcciones de correo electrónico: para obtener una lista de personas.

Se compile la aplicación en varias iteraciones. Con cada iteración, se mejora gradualmente la aplicación. El objetivo de este enfoque de iteración varias es para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el póngase en contacto con el Administrador de la manera más sencilla posible. Se agrega compatibilidad para las operaciones de base de datos básicos: crear, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación sea bonito. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de vista de MVC de ASP.NET predeterminada y en cascada hoja de estilos.

- Iteración #3: agregar validación del formulario. En la tercera iteración, agregamos la validación del formulario básico. Es evitar que los usuarios enviar un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación de acoplamiento flexible. En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que resulten más fáciles de mantener y modificar la aplicación póngase en contacto con el administrador. Por ejemplo, se refactoriza nuestra aplicación para que use el modelo de repositorio y el modelo de inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinto, hacemos nuestra aplicación más fácil de mantener y modificar mediante la adición de pruebas unitarias. Hemos simular nuestro clases del modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración &#6;: Use desarrollo controlado por pruebas. En esta iteración sexto, se agregan nuevas funciones a nuestra aplicación escribiendo pruebas unitarias en primer lugar y escribir código frente a las pruebas unitarias. En esta iteración, agregamos grupos de contactos.

- Iteración #7 - agregar funcionalidad de Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad para Ajax.

## <a name="this-iteration"></a>Esta iteración

El objetivo de esta iteración es mejorar el aspecto de la aplicación póngase en contacto con el administrador. Actualmente, el Administrador de contacto usa la página principal de vista de MVC de ASP.NET predeterminada y hoja de estilos en cascada (consulte la figura 1). Estos don t son incorrectos, pero que no necesita el Administrador de contacto apariencia de cada otro sitio Web de ASP.NET MVC. Desea reemplazar estos archivos con los archivos personalizados.


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figura 01**: la apariencia predeterminada de una aplicación de MVC de ASP.NET ([haga clic aquí para ver la imagen a tamaño completo](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


En esta iteración, describen dos métodos para mejorar el diseño visual de la aplicación. En primer lugar, muestran cómo aprovechar las ventajas de la Galería de diseño de MVC de ASP.NET para descargar una plantilla de diseño de MVC de ASP.NET gratuita. La Galería de diseño de MVC de ASP.NET le permite crear una aplicación web profesional sin tener que realizar ningún trabajo.

Se decidió no utilizar una plantilla de la Galería de diseño de MVC de ASP.NET para la aplicación póngase en contacto con el administrador. En su lugar, tenía un diseño personalizado creado por una empresa de diseño profesional. En la segunda parte de este tutorial, explica cómo trabajé con una empresa de diseño profesional para crear el diseño final de ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>La Galería de diseño de MVC de ASP.NET

La Galería de diseño de MVC de ASP.NET es un recurso gratuito proporcionado por Microsoft. La Galería de MVC de ASP.NET se encuentra en la siguiente dirección:

[https://www.ASP.NET/MVC/Gallery](https://www.asp.net/mvc/gallery)

La Galería de diseño de MVC de ASP.NET hospeda una colección de diseños de sitio Web gratuita que se crearon específicamente para usar en un proyecto de MVC de ASP.NET. Diseños se cargan los miembros de la Comunidad. Los visitantes de la Galería pueden votar por sus diseños favoritos (consulte la figura 2).


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figura 02**: la Galería de diseño de MVC de ASP.NET ([haga clic aquí para ver la imagen a tamaño completo](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


Momento de escribir este tutorial, el diseño más popular en la galería es un diseño denominado octubre por David Hauser. Puede usar este diseño de un proyecto de ASP.NET MVC mediante los siguientes pasos:

1. Haga clic en el **descargar** botón para descargar el archivo October.zip en el equipo.
2. Haga clic en el archivo October.zip descargado y haga clic en el **Unblock** botón (consulte la figura 3).
3. Descomprima el archivo en una carpeta denominada octubre.
4. Seleccionar todos los archivos de la carpeta de DesignTemplate contenida en la carpeta de octubre, haga clic en los archivos y seleccione la opción de menú **copia**.
5. Haga clic en el nodo del proyecto ContactManager en la ventana Explorador de soluciones de Visual Studio y seleccione la opción de menú **pegar** (consulte la figura 4).
6. Seleccione la opción de menú de Visual Studio **editar, buscar y reemplazar, reemplazo rápido** y reemplace *[MyProjectName]* con *ContactManager* (Véase la figura 5).


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figura 03**: desbloquear un archivo descargado de Internet ([haga clic aquí para ver la imagen a tamaño completo](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figura 04**: sobrescribiendo los archivos en el Explorador de soluciones ([haga clic aquí para ver la imagen a tamaño completo](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figura 05**: sustituyendo [ProjectName] ContactManager ([haga clic aquí para ver la imagen a tamaño completo](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


Después de completar estos pasos, la aplicación web usará el nuevo diseño. La página en la figura 6 muestra el aspecto de la aplicación de administrador de contacto con el diseño de octubre.


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figura 06**: ContactManager con la plantilla de octubre ([haga clic aquí para ver la imagen a tamaño completo](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Crear un diseño MVC de ASP.NET personalizados

La Galería de diseño de MVC de ASP.NET tiene una buena selección de estilos de diseño diferentes. La Galería le proporciona un mecanismo sencillo para personalizar la apariencia de las aplicaciones de ASP.NET MVC. Y, por supuesto, la Galería tiene la gran ventaja de estar totalmente gratis.

Sin embargo, deberá crear un diseño completamente único para el sitio Web. En ese caso, tiene sentido para trabajar con una empresa de diseño del sitio Web. Se decidió adoptar este enfoque para el diseño de la aplicación póngase en contacto con el administrador.

Seguridad Contact Manager del iteración #1 y lo envíe el proyecto a la empresa de diseño. No poseen Visual Studio (lástima en ellos!), pero que t Professionals representa un problema. Fueran puede descargar Microsoft Visual Web Developer de forma gratuita desde el [https://www.asp.net](https://www.asp.net) sitio Web y abra la aplicación de administrador de contacto en Visual Web Developer. En un par de días, se hubiera producido el diseño en la figura 7.


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figura 07**: el diseño de administrador de contacto de MVC de ASP.NET ([haga clic aquí para ver la imagen a tamaño completo](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


El nuevo diseño constaba de dos archivos principales: un nuevo archivo de hoja de estilos en cascada y un nuevo archivo de página de vista maestra. Una página maestra de la vista contiene el diseño y el contenido compartido para las vistas en una aplicación ASP.NET MVC. Por ejemplo, la página maestra de vista incluye el encabezado, las pestañas de navegación y pie de página que aparecen en la figura 7. Sobrescribió la Site.Master Vista página principal existente en la carpeta Views\Shared con el nuevo archivo Site.Master de la compañía de diseño

La empresa de diseño también crea una nueva hoja de estilos en cascada y un conjunto de imágenes. Colocan estos archivos nuevos en la carpeta de contenido y se sobrescribió el archivo Site.css existente. Debería colocar todo el contenido estático en la carpeta de contenido.

Observe que el nuevo diseño para el Administrador de contacto incluye imágenes para modificar y eliminar contactos. Una imagen de editar y eliminar aparecen al lado de cada contacto de la tabla HTML de contactos.

Originalmente, estos vínculos se representan con el código HTML. Aplicación auxiliar de ActionLink() similar al siguiente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

El método Html.ActionLink() no admite imágenes (el método HTML codifica el texto del vínculo por motivos de seguridad). Por lo tanto, las llamadas a Html.ActionLink() reemplazan por llamadas a Url.Action() similar al siguiente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

El método de Html.ActionLink() representa un hipervínculo HTML completo. El método Url.Action(), por otro lado, representa la dirección URL sin el &lt;un&gt; etiqueta.

Además, tenga en cuenta que el nuevo diseño incluye pestañas seleccionadas y no seleccionadas. Por ejemplo, en la figura 8, el **crear nuevo contacto** ficha está seleccionada y el **Mis contactos** ficha no está seleccionada.


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figura 08**: seleccionada y anular la selección de las fichas ([haga clic aquí para ver la imagen a tamaño completo](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


Para admitir la representación pestañas seleccionadas y no seleccionadas, creé una aplicación auxiliar HTML personalizada con el nombre de la MenuItemHelper. Este método auxiliar representa ya sea un &lt;li&gt; etiqueta o una &lt;clase li = "activada"&gt; etiqueta en función de si el controlador actual y la acción se corresponde con el nombre de acción y controlador que se pasa a la aplicación auxiliar. El código para el MenuItemHelper se encuentra en la lista 1.

**Lista 1 - Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

El MenuItemHelper utiliza la clase TagBuilder internamente para generar la &lt;li&gt; etiqueta HTML. La clase TagBuilder es una clase de utilidad muy útil que puede usar siempre que tenga que crear una nueva etiqueta HTML. Incluye métodos para agregar atributos, agregar clases CSS, generar identificadores y modificar la etiqueta s HTML interno.

## <a name="summary"></a>Resumen

En esta iteración, se ha mejorado el diseño visual de nuestra aplicación de ASP.NET MVC. En primer lugar, se introdujeron en la Galería de diseño de MVC de ASP.NET. Aprendió a descargar las plantillas de diseño libre de la Galería de diseño de MVC de ASP.NET que puede usar en las aplicaciones de ASP.NET MVC.

A continuación, explicamos cómo puede crear un diseño personalizado modificando el archivo de hoja de estilos en cascada de forma predeterminada y el archivo de página de vista maestra. Para admitir el nuevo diseño, tuvimos que realizar algunos cambios menores en nuestra aplicación póngase en contacto con el administrador. Por ejemplo, hemos agregado una nueva aplicación auxiliar HTML con el nombre de la MenuItemHelper que muestra pestañas seleccionadas y no seleccionadas.

En la siguiente iteración, se abordar al asunto muy importante de validación. Agregamos código de validación a la aplicación para que un usuario no puede crear un nuevo contacto sin proporcionar valores necesarios, como una persona s primero y apellidos.

>[!div class="step-by-step"]
[Anterior](iteration-1-create-the-application-cs.md)
[Siguiente](iteration-3-add-form-validation-cs.md)
