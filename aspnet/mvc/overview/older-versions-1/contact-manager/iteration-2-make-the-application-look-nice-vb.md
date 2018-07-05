---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: 'Iteración #2: que la aplicación parezca interesante (VB) | Microsoft Docs'
author: microsoft
description: En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: c1209a925a43bd7846a9dc07ce557c55bb1827ae
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381110"
---
<a name="iteration-2--make-the-application-look-nice-vb"></a>Iteración #2: que la aplicación parezca interesante (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargue el código](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de ASP.NET MVC (VB) de administración de contactos
  

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa desde el principio para finalizar. La aplicación de Contact Manager le permite almacenar información de contacto: los nombres, direcciones de correo electrónico y números de teléfono: para obtener una lista de personas.

Creamos la aplicación a través de varias iteraciones. Con cada iteración, mejorar gradualmente la aplicación. Es el objetivo de este enfoque de varias iteraciones para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el Administrador de contactos de la manera más sencilla posible. Se agrega compatibilidad con las operaciones de base de datos básicos: creación, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación parezca interesante. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.

- Iteración #3: agregar una validación de formulario. En la tercera iteración, se agrega una validación de formulario básico. Evitamos personas desde el envío de un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación tenga un acoplamiento. En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que sea más fácil de mantener y modificar la aplicación de Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y la inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinta, realizamos nuestra aplicación más fácil de mantener y modificar mediante la adición de las pruebas unitarias. Hemos simular nuestras clases de modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración #6: usar el desarrollo controlado por pruebas. En esta iteración sexta, se agregan nuevas funciones a nuestra aplicación escribir pruebas unitarias en primer lugar y escribir código en las pruebas unitarias. En esta iteración, se agrega grupos de contactos.

- Iteración #7: agregar funcionalidad de Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

El objetivo de esta iteración es mejorar la apariencia de la aplicación de Contact Manager. Actualmente, el Administrador de contactos usa la página principal de la vista de MVC de ASP.NET de forma predeterminada y la hoja de estilos en cascada (consulte la figura 1). Estos don t aspecto negativo, pero que no necesita el Administrador de contacto para el aspecto de cada otro sitio Web de ASP.NET MVC. Quiero reemplazar estos archivos con los archivos personalizados.


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Figura 01**: la apariencia predeterminada de una aplicación ASP.NET MVC ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image2.png))


En esta iteración, describen dos enfoques para mejorar el diseño visual de nuestra aplicación. En primer lugar, mostraré cómo aprovechar las ventajas de la Galería de diseño de MVC de ASP.NET para descargar una plantilla de diseño de MVC de ASP.NET gratuita. La Galería de diseño de MVC de ASP.NET le permite crear una aplicación web profesionales sin realizar ningún trabajo.

Decidido no usar una plantilla desde la Galería de diseño de MVC de ASP.NET para la aplicación de Contact Manager. En su lugar, tuve un diseño personalizado creado por una empresa de diseño profesional. En la segunda parte de este tutorial, explica cómo trabajé con una empresa de diseño profesional para crear el diseño final de ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>La Galería de diseño de MVC de ASP.NET

La Galería de diseño de MVC de ASP.NET es un recurso gratuito proporcionado por Microsoft. La Galería de MVC de ASP.NET se encuentra en la siguiente dirección:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

La Galería de diseño de MVC de ASP.NET hospeda una colección de diseños de sitio Web gratuito que se crearon específicamente para usar en un proyecto de ASP.NET MVC. Diseños se cargan los miembros de la Comunidad. Los visitantes de la Galería pueden votar por sus diseños favoritos (consulte la figura 2).


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Figura 02**: la Galería de diseño de MVC de ASP.NET ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image4.png))


Mientras escribo este tutorial, el diseño más popular en la galería es un diseño denominado octubre por David Hauser. Puede usar este diseño para un proyecto ASP.NET MVC mediante los pasos siguientes:

1. Haga clic en el **descargar** botón para descargar el archivo October.zip en el equipo.
2. Haga clic en el archivo October.zip descargado y haga clic en el **desbloquear** (consulte la figura 3).
3. Descomprima el archivo en una carpeta denominada octubre.
4. Seleccione todos los archivos de la carpeta DesignTemplate contenida en la carpeta de octubre, haga clic en los archivos y seleccione la opción de menú **copia**.
5. Haga clic en el nodo del proyecto ContactManager en la ventana Explorador de soluciones de Visual Studio y seleccione la opción de menú **pegar** (consulte la figura 4).
6. Seleccione la opción de menú de Visual Studio **editar, buscar y reemplazar, reemplazo rápido** y reemplace *[MyProjectName]* con *ContactManager* (consulte la figura 5).


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Figura 03**: desbloquear un archivo descargado de Internet ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image6.png))


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Figura 04**: sobrescribiendo los archivos en el Explorador de soluciones ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image8.png))


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Figura 05**: sustituyendo [ProjectName] ContactManager ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image10.png))


Después de completar estos pasos, la aplicación web usará el nuevo diseño. La página en la figura 6 muestra la apariencia de la aplicación de Contact Manager con el diseño de octubre.


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Figura 06**: ContactManager con la plantilla de octubre ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Crear un diseño MVC de ASP.NET personalizados

La Galería de diseño de MVC de ASP.NET tiene una buena selección de los estilos de diseño diferentes. La Galería proporciona una manera sencilla para personalizar la apariencia de las aplicaciones de ASP.NET MVC. Y, por supuesto, la Galería tiene la gran ventaja de estar totalmente gratuita.

Sin embargo, es posible que deba crear un diseño completamente único para el sitio Web. En ese caso, tiene sentido para trabajar con una empresa de diseño del sitio Web. Decidido adoptar este enfoque para el diseño de la aplicación de Contact Manager.

De Contact Manager de iteración 1 y lo envíe el proyecto a la empresa de diseño. No poseen Visual Studio (es lamentable en ellas!), pero que no ha t presentan un problema. Podían descargar Microsoft Visual Web Developer de forma gratuita desde el [ https://www.asp.net ](https://www.asp.net) sitio Web y abra la aplicación de Contact Manager en Visual Web Developer. En un par de días, se hubiera producido el diseño en la figura 7.


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Figura 07**: el diseño de ASP.NET MVC Contact Manager ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image14.png))


El nuevo diseño del consistió en dos archivos principales: un nuevo archivo de hoja de estilos en cascada y un nuevo archivo de página de vista maestra. Una página maestra de la vista contiene el diseño y el contenido compartido de las vistas en una aplicación ASP.NET MVC. Por ejemplo, la página principal de la vista incluye el encabezado, las pestañas de navegación y pie de página que aparecen en la figura 7. Sobrescribió el Site.Master Vista página principal existente en la carpeta Views\Shared con el nuevo archivo Site.Master de la compañía de diseño

La compañía de diseño también se crea una nueva hoja de estilos en cascada y un conjunto de imágenes. Puedo colocar estos nuevos archivos en la carpeta de contenido y sobrescribió el archivo Site.css existente. Debe colocar todo el contenido estático en la carpeta de contenido.

Observe que el nuevo diseño de Contact Manager incluye imágenes para modificar y eliminar contactos. Una imagen de edición y eliminación aparecen al lado de cada contacto de la tabla HTML de contactos.

Originalmente, estos vínculos se representaran con el código HTML. Aplicación auxiliar de ActionLink() similar al siguiente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

El método Html.ActionLink() no admite imágenes (el método HTML codifica el texto del vínculo por motivos de seguridad). Por lo tanto, reemplazar las llamadas a Html.ActionLink() con llamadas a Url.Action() similar al siguiente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

El método de Html.ActionLink() representa un hipervínculo HTML completo. El método Url.Action(), por otro lado, representa la dirección URL sin el &lt;un&gt; etiqueta.

Además, tenga en cuenta que el nuevo diseño incluye pestañas seleccionadas o sin seleccionadas. Por ejemplo, en la figura 8, el **crear nuevo contacto** pestaña está seleccionada y el **Mis contactos** ficha no está seleccionada.


[![El cuadro de diálogo nuevo proyecto](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Figura 08**: activada y desactivada pestañas ([haga clic aquí para ver imagen en tamaño completo](iteration-2-make-the-application-look-nice-vb/_static/image16.png))


Para poder representar pestañas seleccionadas o sin seleccionadas, he creado una aplicación auxiliar HTML personalizada denominada el MenuItemHelper. Este método auxiliar representa cualquiera un &lt;li&gt; etiqueta o una &lt;clase li = "selected"&gt; etiqueta en función de si el controlador actual y la acción se corresponde con el nombre de acción y controlador pasado a la aplicación auxiliar. El código para el MenuItemHelper está contenido en el listado 1.

**Listado 1 – Helpers\MenuItemHelper.vb**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

El MenuItemHelper usa la clase TagBuilder internamente para crear el &lt;li&gt; etiqueta HTML. La clase TagBuilder es una clase de utilidad muy útil que puede usar siempre que tenga que crear una nueva etiqueta HTML. Incluye métodos para agregar atributos, agregar clases CSS, generación de identificadores y modificación de la etiqueta a la s HTML interno.

## <a name="summary"></a>Resumen

En esta iteración, se ha mejorado el diseño visual de nuestra aplicación de ASP.NET MVC. En primer lugar, se introdujeron en la Galería de diseño de MVC de ASP.NET. Ha aprendido cómo descargar las plantillas de diseño gratuita desde la Galería de diseño de MVC de ASP.NET que puede usar en sus aplicaciones de ASP.NET MVC.

A continuación, explicamos cómo puede crear un diseño personalizado modificando el archivo de hoja de estilos en cascada de forma predeterminada y el archivo de página de vista maestra. Para admitir el nuevo diseño, se tenía que realizar algunos cambios menores a nuestra aplicación de Contact Manager. Por ejemplo, hemos agregado una nueva aplicación auxiliar HTML denominado el MenuItemHelper que muestra pestañas seleccionadas o sin seleccionadas.

En la siguiente iteración, abordamos al asunto muy importante de validación. Agregamos el código de validación a nuestra aplicación para que un usuario no puede crear un nuevo contacto sin proporcionar valores necesarios, como una persona s primero y el apellido.

> [!div class="step-by-step"]
> [Anterior](iteration-1-create-the-application-vb.md)
> [Siguiente](iteration-3-add-form-validation-vb.md)
