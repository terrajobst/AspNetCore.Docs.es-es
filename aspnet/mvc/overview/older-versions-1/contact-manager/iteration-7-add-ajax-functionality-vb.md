---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Iteración #7: agregar funcionalidad de Ajax (VB) | Microsoft Docs'
author: microsoft
description: En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad con Ajax.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: 0b9c6ff228e73ce63f7a0b046110db656103d6d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836073"
---
<a name="iteration-7--add-ajax-functionality-vb"></a>Iteración #7: agregar funcionalidad de Ajax (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargue el código](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad con Ajax.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de ASP.NET MVC (VB) de administración de contactos
  

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa desde el principio para finalizar. La aplicación de Contact Manager permite almacenar información de contacto - nombres, números de teléfono y direcciones de correo electrónico: para obtener una lista de personas.

Creamos la aplicación a través de varias iteraciones. Con cada iteración, mejorar gradualmente la aplicación. Es el objetivo de este enfoque de varias iteraciones para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el Administrador de contactos de la manera más sencilla posible. Se agrega compatibilidad con las operaciones de base de datos básicos: creación, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación parezca interesante. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.

- Iteración #3: agregar validación de formulario. En la tercera iteración, se agrega una validación de formulario básico. Evitamos personas desde el envío de un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación tenga un acoplamiento. En este cuarta iteración, aprovechamos de varios patrones de diseño de software para que sea más fácil de mantener y modificar la aplicación de Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y la inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinta, realizamos nuestra aplicación más fácil de mantener y modificar mediante la adición de las pruebas unitarias. Hemos simular nuestras clases de modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración #6: usar el desarrollo controlado por pruebas. En esta iteración sexta, se agregan nuevas funciones a nuestra aplicación escribir pruebas unitarias en primer lugar y escribir código en las pruebas unitarias. En esta iteración, se agrega grupos de contactos.

- Iteración #7: agregar funcionalidad Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En esta iteración de la aplicación de Contact Manager, se refactoriza la aplicación para hacer uso de Ajax. Aprovechando las ventajas de Ajax, nos que nuestra aplicación responda mejor. Podemos evitar toda una página de representación cuando sea necesario actualizar solo una región determinada en una página.

Se deberá refactorizar nuestra vista índice para que no queremos t necesidad de volver a mostrar la página completa cada vez que un usuario selecciona un nuevo grupo de contactos. En su lugar, cuando un usuario hace clic en un grupo de contactos, que actualizar la lista de contactos y deje el resto de la página por sí solo.

También cambiará la forma de nuestro Eliminar vínculo funciona. En lugar de mostrar una página de confirmación independiente, se mostrará un cuadro de diálogo de confirmación de JavaScript. Si el usuario confirma que desea eliminar un contacto, se realiza una operación DELETE de HTTP en el servidor para eliminar el registro de contacto de la base de datos.

Además, se aprovechará de jQuery para agregar efectos de animación a la vista de índice. Se mostrará una animación cuando la nueva lista de contactos se recuperan desde el servidor.

Por último, tomamos la ventaja de la compatibilidad del marco de AJAX de ASP.NET para administrar el historial del explorador. Vamos a crear los puntos del historial cada vez que se realice una llamada de Ajax para actualizar la lista de contactos. De este modo, el explorador hacia atrás y hacia delante botones will funcione.

## <a name="why-use-ajax"></a>¿Por qué usar Ajax?

Usar Ajax tiene muchas ventajas. En primer lugar, puede agregar funcionalidad de Ajax en el resultado de una mejor experiencia de usuario de una aplicación. En una aplicación web normal, toda la página debe se publican en el servidor cada vez que un usuario realiza una acción. Cada vez que realice una acción, los bloqueos de explorador y el usuario deben esperar hasta que se capturen y se vuelve a mostrar la página completa.

Esto sería una experiencia inaceptable en el caso de una aplicación de escritorio. Sin embargo, hemos vivido tradicionalmente, con esta experiencia de usuario incorrectos en el caso de una aplicación web porque no se sabía que podríamos hacer mejor. Pensamos que era una limitación de las aplicaciones web cuando en realidad, era simplemente una limitación de nuestro imaginación.

En una aplicación Ajax, que no la necesidad de t para llevar la experiencia de usuario detenerse sólo para actualizar una página. En su lugar, puede realizar una solicitud asincrónica en segundo plano para actualizar la página. Cuando no fuerza t al usuario que espere mientras se actualiza la parte de la página.

Aprovechando las ventajas de Ajax, también se puede mejorar el rendimiento de la aplicación. Considere la posibilidad de funcionamiento de la aplicación de Contact Manager ahora mismo sin la funcionalidad de Ajax. Al hacer clic en un grupo de contactos, toda la vista de índice debe volverá a mostrar. La lista de contactos y la lista de grupos de contactos se deben recuperar desde el servidor de base de datos. Todos estos datos deben pasarse a través de la conexión de servidor web al explorador web.

Sin embargo, después, agregar funcionalidad Ajax a nuestra aplicación, podemos evitar volver a mostrar toda la página cuando un usuario hace clic en un grupo de contactos. Ya no se necesita tomar los grupos de contactos de la base de datos. También no queremos necesidad t para insertar la vista de índice completa a través de la conexión. Aprovechando las ventajas de Ajax, se reduce la cantidad de trabajo que debe realizar nuestro servidor de base de datos y se reduce la cantidad de tráfico de red requerido por nuestra aplicación.

## <a name="don-t-be-afraid-of-ajax"></a>Don t tener miedo de Ajax

Algunos desarrolladores evite usar Ajax porque les preocupa de los exploradores. Quieren asegurarse de que sus aplicaciones web seguirá funcionando cuando obtiene acceso a un explorador que no es compatible con JavaScript. Puesto que depende de Ajax en JavaScript, algunos desarrolladores evite el uso de Ajax.

Sin embargo, si tiene cuidado acerca de cómo implementar Ajax, a continuación, puede crear aplicaciones que funcionan con los exploradores de nivel superior y de nivel inferior. Nuestra aplicación de Contact Manager funcionarán con los exploradores que admiten JavaScript y los exploradores que no lo hacen.

Si usa la aplicación de Contact Manager con un explorador que admita JavaScript tendrá una mejor experiencia de usuario. Por ejemplo, al hacer clic en un grupo de contactos, se actualizará solo en la región de la página que muestra los contactos.

Si, por otro lado, usar la aplicación de Contact Manager con un explorador que no es compatible con JavaScript (o que tiene JavaScript deshabilitado) tendrá una experiencia de usuario ligeramente menos deseable. Por ejemplo, al hacer clic en un grupo de contactos, se debe registrar toda la vista de índice al explorador para mostrar la lista de contactos coincidentes.

## <a name="adding-the-required-javascript-files"></a>Agregar los archivos JavaScript necesarios

Deberá usar los tres archivos de JavaScript para agregar funcionalidad Ajax a nuestra aplicación. Tres de estos archivos se incluyen en la carpeta de Scripts de una nueva aplicación de ASP.NET MVC.

Si tiene previsto usar Ajax en varias páginas de la aplicación, a continuación, tiene sentido incluir los archivos JavaScript necesarios en la página de aplicación s vista maestra. De este modo, los archivos JavaScript se incluirán en todas las páginas en la aplicación automáticamente.

Agregue el siguiente código de JavaScript que se incluye dentro de la &lt;head&gt; etiqueta de la página principal de la vista:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactorización de la vista de índice para usar Ajax

Permiten s comenzamos por modificar la vista de índice para que al hacer clic en un grupo de contactos solo actualiza la región de la vista que muestra los contactos. El cuadro rojo en la figura 1 contiene el área que se va a actualizar.


[![Actualizar solo los contactos](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Figura 01**: actualizar solo los contactos ([haga clic aquí para ver imagen en tamaño completo](iteration-7-add-ajax-functionality-vb/_static/image2.png))


El primer paso es separar la parte de la vista que se va a actualizar de forma asincrónica en una parcial independiente (control de usuario de vista). La sección de la vista de índice que se muestra en la tabla de contactos se ha movido a la parcial en el listado 1.

**Listado 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Tenga en cuenta que la parcial en el listado 1 tiene un modelo diferente de la vista de índice. El *Inherits* atributo en el &lt;% @ Page %&gt; directiva especifica que la parcial se hereda el ViewUserControl&lt;grupo&gt; clase.

La vista de índice actualizada se incluye en el listado 2.

**Listado 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Hay dos cosas que debe tener en cuenta acerca de la vista actualizada en el listado 2. En primer lugar, tenga en cuenta que todo el contenido que se mueven a la parcial se reemplaza con una llamada a Html.RenderPartial(). El método Html.RenderPartial() se llama cuando se solicita la vista de índice por primera vez con el fin de mostrar el conjunto inicial de contactos.

En segundo lugar, tenga en cuenta que se ha reemplazado el Html.ActionLink() utilizado para mostrar los grupos de contactos con un Ajax.ActionLink(). Se llama a la Ajax.ActionLink() con los siguientes parámetros:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

El primer parámetro representa el texto que se muestra en el vínculo, el segundo parámetro representa los valores de ruta y el tercer parámetro representa las opciones de Ajax. En este caso, usamos la opción de UpdateTargetId Ajax para que apunte al HTML &lt;div&gt; etiqueta que se va a actualizar una vez finalizada la solicitud de Ajax. Desea actualizar el &lt;div&gt; etiqueta con la nueva lista de contactos.

El método Index() actualizado del controlador de contacto se encuentra en el listado 3.

**Listado 3 - Controllers\ContactController.vb (método de Index)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

La acción de Index() actualizada condicionalmente devuelve uno de dos cosas. Si se invoca la acción de Index() por una solicitud Ajax, a continuación, el controlador devuelve un parcial. En caso contrario, la acción de Index() devuelve una vista completa.

Tenga en cuenta que la acción de Index() no es necesario devolver todos los datos cuando el usuario invoca una solicitud Ajax. En el contexto de una solicitud normal, la acción del índice devuelve una lista de todos los grupos de contactos y el grupo de contactos seleccionado. En el contexto de una solicitud Ajax, la acción de Index() devuelve solo el grupo seleccionado. AJAX le supone menos trabajo en el servidor de base de datos.

Nuestra vista índice modificado funciona en el caso de los exploradores de nivel superior y de nivel inferior. Si hace clic en un grupo de contactos, y el explorador admite JavaScript, se actualiza únicamente la región de la vista que contiene la lista de contactos. Si, por otro lado, el explorador no es compatible con JavaScript, se actualiza toda la vista.


Nuestra vista actualizada de índice tiene un problema. Al hacer clic en un grupo de contactos, el grupo seleccionado no está resaltado. Como se muestra la lista de grupos fuera de la región que se actualiza durante una solicitud Ajax, no obtener resaltado el grupo adecuado. Este problema se corregirá en la sección siguiente.


## <a name="adding-jquery-animation-effects"></a>Adición de efectos de animación de jQuery

Normalmente, al hacer clic en un vínculo en una página web, puede usar la barra de progreso del explorador para detectar si el explorador está obteniendo activamente el contenido actualizado. Por otro lado, al realizar una solicitud Ajax, la barra de progreso del explorador no muestra ningún progreso. Esto puede hacer que los usuarios nervioso. ¿Cómo sé si se ha inmovilizado el explorador?

Hay varias formas que puede indicar a un usuario que se está realizando el trabajo mientras se realiza una solicitud Ajax. Un enfoque consiste en mostrar una animación sencilla. Por ejemplo, puede atenuar una región cuando comienza una solicitud Ajax y una atenuación en la región cuando se completa la solicitud.

Vamos a usar la biblioteca de jQuery que se incluye con el marco de MVC de ASP.NET de Microsoft, para crear los efectos de animación. La vista de índice actualizada se incluye en el listado 4.

**Listado 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Observe que la vista de índice actualizada contiene tres nuevas funciones de JavaScript. Las dos primeras funciones usan jQuery para atenuar y atenuar en la lista de contactos al hacer clic en un nuevo grupo de contactos. La tercera función muestra un mensaje de error cuando una solicitud Ajax, tiene como resultado un error (por ejemplo, tiempo de espera de red).

La primera función también se encarga de resaltado el grupo seleccionado. Una clase = atributo seleccionado se agrega al elemento primario (el elemento LI) del elemento que se hizo clic. Nuevamente, jQuery facilita seleccionar el elemento correcto y agregar la clase CSS.

Estas secuencias de comandos están vinculados a los vínculos de grupo con la Ayuda del parámetro Ajax.ActionLink() AjaxOptions. La llamada al método Ajax.ActionLink() actualizado tiene este aspecto:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Adición de compatibilidad de historial del explorador

Normalmente, al hacer clic en un vínculo para actualizar una página, se actualiza el historial del explorador. De este modo, puede hacer clic en el botón Atrás del explorador para retroceder en el tiempo al estado anterior de la página. Por ejemplo, si hace clic en el grupo de contactos de sus amigos y, a continuación, haga clic en el grupo de contactos de negocio, hacer clic en el botón Atrás del explorador para navegar al estado de la página cuando se ha seleccionado el grupo de contactos de sus amigos.

Por desgracia, realizar una solicitud de Ajax no actualizar historial del explorador automáticamente. Si hace clic en un grupo de contactos, y se recupera la lista de contactos coincidentes con una solicitud Ajax, no se actualiza el historial del explorador. No se puede usar el botón Atrás del explorador para volver a un grupo de contactos después de seleccionar un nuevo grupo de contactos.

Si desea que los usuarios puedan usar el explorador vuelva botón después de realizar las solicitudes Ajax, a continuación, deberá realizar algo más de trabajo. Se necesita aprovechar la funcionalidad de administración del historial de explorador integrada en el marco de AJAX de ASP.NET.

Historial del explorador de AJAX de ASP.NET, es preciso hacer tres cosas:

1. Habilitar el historial del explorador estableciendo la propiedad enableBrowserHistory en true.
2. Guardar los puntos del historial cuando cambia el estado de una vista llamando al método addHistoryPoint().
3. Reconstruir el estado de la vista cuando se provoca el evento de navegación.

La vista de índice actualizada se incluye en el listado 5.

**Listado 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

En el listado 5, el historial del explorador está habilitado en la función pageInit(). La función pageInit() también se utiliza para configurar el controlador de eventos para el evento de navegación. Se produce el evento navigate cada vez que el explorador hacia delante o atrás hace que el estado de la página para cambiar.

Se llama al método de beginContactList() al hacer clic en un grupo de contactos. Este método crea un nuevo punto del historial llamando al método addHistoryPoint(). El identificador de hacer clic en el grupo de contactos se agrega al historial.

El identificador de grupo se recupera de un atributo expando en el vínculo del grupo de contactos. El vínculo se representa con la siguiente llamada a Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

El último parámetro pasado a la Ajax.ActionLink() agrega un atributo expando denominado groupid al vínculo (en minúsculas para la compatibilidad XHTML).

Cuando un usuario presiona el botón de hacia delante o Atrás del explorador, se genera el evento de navegación y se llama al método navigate(). Este método actualiza los contactos que aparecen en la página para que coincida con el estado de la página que se corresponde con el punto del historial de explorador pasado al método navigate.

## <a name="performing-ajax-deletes"></a>Realización de Ajax elimina

Actualmente, con el fin de eliminar un contacto, debe hacer clic en el vínculo Eliminar y, a continuación, haga clic en el botón Eliminar que se muestra en la página de confirmación de eliminación (consulte la figura 2). Esto parece una gran cantidad de solicitudes de página en algo tan simple como la eliminación de un registro de base de datos.


[![La página de confirmación de eliminación](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Figura 02**: la página de confirmación de eliminación ([haga clic aquí para ver imagen en tamaño completo](iteration-7-add-ajax-functionality-vb/_static/image4.png))


Es tentador para omitir la página de confirmación de borrado y eliminación de un contacto directamente desde la vista de índice. Debe evitar la tentación porque este enfoque abre la aplicación a vulnerabilidades de seguridad. En general, don t desee realizar una operación HTTP GET al invocar una acción que modifica el estado de la aplicación web. Al realizar una eliminación, que desea realizar una solicitud HTTP POST o, mejor aún, una operación DELETE de HTTP.

El vínculo Eliminar está contenido en el ContactList parcial. Una versión actualizada de la ContactList parcial se encuentra en el listado 6.

**Listado 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

El vínculo Eliminar se representa con la siguiente llamada al método Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> El Ajax.ImageActionLink() no es una parte estándar del marco ASP.NET MVC. El Ajax.ImageActionLink() es un método auxiliar personalizada incluido en el proyecto de Contact Manager.


El parámetro AjaxOptions tiene dos propiedades. En primer lugar, la propiedad confirmar se utiliza para mostrar un cuadro de diálogo de confirmación emergente JavaScript. En segundo lugar, la propiedad HttpMethod se usa para realizar una operación DELETE de HTTP.

Listado 7 contiene una nueva acción AjaxDelete() que se ha agregado al controlador de contacto.

**Listado 7 - Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

La acción AjaxDelete() está decorada con un atributo AcceptVerbs. Este atributo se evita que la acción que se invoca, excepto por cualquier operación de HTTP que no sea una operación DELETE de HTTP. En concreto, no se puede invocar esta acción con un HTTP GET.

Después de eliminar el registro de base de datos, debe mostrar la lista actualizada de contactos que no contiene el registro eliminado. El método AjaxDelete() devuelve el ContactList parcial y la lista actualizada de contactos.

## <a name="summary"></a>Resumen

En esta iteración, hemos agregado la funcionalidad Ajax a nuestra aplicación de Contact Manager. Se usa Ajax para mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación.

En primer lugar, hemos refactorizado la vista de índice para que al hacer clic en un grupo de contactos no se actualiza toda la vista. En su lugar, al hacer clic en un grupo de contactos solo actualiza la lista de contactos.

A continuación, hemos usado los efectos de animación de jQuery para atenuar y atenuar en la lista de contactos. Agregar animación a una aplicación Ajax puede utilizarse para proporcionar a los usuarios de la aplicación con el equivalente de una barra de progreso del explorador.

También hemos agregado compatibilidad de historial del explorador a nuestra aplicación de Ajax. Se habilita a los usuarios haga clic en el explorador vuelva y reenviar los botones para cambiar el estado de la vista de índice.

Por último, se crea un vínculo de eliminación que admite operaciones HTTP DELETE. Mediante la realización de eliminaciones de Ajax, permitimos que los usuarios eliminar registros de base de datos sin necesidad de que el usuario solicitar una página de confirmación de eliminación adicionales.

> [!div class="step-by-step"]
> [Anterior](iteration-6-use-test-driven-development-vb.md)
