---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: "Iteración #7: la funcionalidad de Ajax de agregar (C#) | Documentos de Microsoft"
author: microsoft
description: "En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad para Ajax."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: db313d12dfd6a146347f253dc3a1f4a889bee780
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="iteration-7--add-ajax-functionality-c"></a>Iteración #7: la funcionalidad de Ajax de agregar (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad para Ajax.


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

En esta iteración de la aplicación Administrador de contacto, se refactoriza nuestra aplicación hacer uso de Ajax. Aprovechando las ventajas de Ajax, hacemos nuestra aplicación mayor capacidad de respuesta. Podemos evitar representar una página completa cuando se necesita actualizar solo una región determinada en una página.

La vista de índice se podrá refactorizar para que no queremos t deba volver a mostrar la página completa cada vez que un usuario selecciona un nuevo grupo de contactos. En su lugar, cuando un usuario hace clic en un grupo de contactos, comenzaremos simplemente actualizar la lista de contactos y deje el resto de la página por sí sola.

También se podrá cambiar la manera nuestro Eliminar vínculo funciona. En lugar de mostrar una página de confirmación independiente, se mostrará un cuadro de diálogo de confirmación de JavaScript. Si lo confirma que desea eliminar un contacto, se realiza una operación DELETE de HTTP en el servidor para eliminar el registro de contacto de la base de datos.

Además, se aprovechará de jQuery para agregar efectos de animación a la vista de índice. Se mostrará una animación cuando la nueva lista de contactos se han recuperado desde el servidor.

Por último, podrá beneficiarse de la compatibilidad de marco de AJAX de ASP.NET para administrar el historial del explorador. Vamos a crear los puntos del historial siempre que se realiza una llamada Ajax para actualizar la lista de contactos. De este modo, el explorador hacia atrás y hacia delante botones will funcione.

## <a name="why-use-ajax"></a>¿Por qué usar Ajax?

El uso de Ajax tiene muchas ventajas. En primer lugar, puede agregar funcionalidad de Ajax en el resultado de una mejor experiencia de usuario de una aplicación. En una aplicación web normal, la página completa debe enviarse al servidor cada vez que un usuario realiza una acción. Cada vez que realice una acción, los bloqueos de explorador y el usuario deben esperar hasta que se capturan y se vuelve a mostrar toda la página.

Esto sería una experiencia de inaceptable en el caso de una aplicación de escritorio. Sin embargo, normalmente, se duración con esta experiencia de usuario incorrectos en el caso de una aplicación web porque no se supiera que podríamos hacer mejor. Pensábamos que era una limitación de las aplicaciones web en los que, en realidad, es simplemente una limitación de nuestro imaginación.

En una aplicación Ajax, que no t necesario para hacer que la experiencia del usuario detenga solo para actualizar una página. En su lugar, puede realizar una solicitud asincrónica en segundo plano para actualizar la página. Cuando no fuerza de t al usuario que espere mientras se actualiza la parte de la página.

Aprovechando las ventajas de Ajax, también puede mejorar el rendimiento de la aplicación. Considere la posibilidad de funcionamiento de la aplicación Administrador contacto ahora sin la funcionalidad de Ajax. Al hacer clic en un grupo de contactos, toda la vista de índice debe volver a mostrarse. La lista de contactos y la lista de grupos de contactos se deben recuperar desde el servidor de base de datos. Todos estos datos deben pasarse a través de la conexión de servidor web en el explorador web.

Sin embargo, después de que se agregue la funcionalidad de Ajax a nuestra aplicación, podemos evitar volver a mostrar toda la página cuando un usuario hace clic en un grupo de contactos. Ya no es necesario obtener los grupos de contactos de la base de datos. También no queremos t necesario distribuir toda la vista de índice en línea. Aprovechando las ventajas de Ajax, se reduce la cantidad de trabajo que debe realizar en el servidor de base de datos y se reduce la cantidad de tráfico de red requerido por la aplicación.

## <a name="don-t-be-afraid-of-ajax"></a>Don t ser atreve de Ajax

Algunos desarrolladores evitar el uso de Ajax porque preocuparse de los exploradores. Desean asegurarse de que sus aplicaciones web seguirán funcionando cuando obtiene acceso a un explorador que no admite JavaScript. Dado que Ajax depende de JavaScript, algunos desarrolladores evite usar Ajax.

Sin embargo, si tiene cuidado acerca de cómo implementar Ajax puede compilar aplicaciones que funcionan con exploradores de nivel superior y de nivel inferior. Nuestra aplicación póngase en contacto con el administrador funcionarán con los exploradores que admiten JavaScript y exploradores que no lo hacen.

Si usa la aplicación Administrador de contacto con un explorador que admita JavaScript tendrá una mejor experiencia de usuario. Por ejemplo, al hacer clic en un grupo de contactos, se actualizará solo la región de la página que muestra los contactos.

Si, por otro lado, se utiliza la aplicación Administrador de contacto con un explorador que no admite JavaScript (o que tenga JavaScript deshabilitado), tendrá una experiencia de usuario deseable ligeramente inferior. Por ejemplo, al hacer clic en un grupo de contactos, toda la vista de índice debe enviarse volver al explorador con el fin de mostrar la lista de búsqueda de coincidencias de contactos.

## <a name="adding-the-required-javascript-files"></a>Agregar los archivos JavaScript necesarios

Necesitaremos usar tres archivos de JavaScript para agregar funcionalidad Ajax a nuestra aplicación. Las tres de estos archivos se incluyen en la carpeta de Scripts de una nueva aplicación MVC de ASP.NET.

Si tiene previsto usar Ajax en varias páginas de la aplicación, a continuación, tiene sentido incluir los archivos JavaScript necesarios en la página de aplicación s vista maestra. De este modo, los archivos JavaScript se incluirá en todas las páginas en la aplicación automáticamente.

Agregue el siguiente código de JavaScript que se incluye dentro de la &lt;head&gt; etiqueta de la página principal de vista:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactorización de la vista de índice para usar Ajax

Permiten s iniciar mediante la modificación de la vista de índice para que solo se hace clic en un grupo de contactos actualiza la región de la vista que muestra los contactos. El cuadro rojo en la figura 1 contiene la región que se va a actualizar.


[![Actualizar solo los contactos](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Figura 01**: actualizar solo contactos ([haga clic aquí para ver la imagen a tamaño completo](iteration-7-add-ajax-functionality-cs/_static/image2.png))


El primer paso es separar la parte de la vista que se va a actualizar de forma asincrónica en un parcial independiente (control de vista de usuario). Se ha movido la sección de la vista de índice que muestra la tabla de contactos en parte en la lista 1.

**Lista 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Observe que la parcial en la lista 1 tiene un modelo diferente de la vista de índice. El *Inherits* de atributo en el &lt;% @ Page %&gt; directiva especifica que la parcial hereda de la ViewUserControl&lt;grupo&gt; clase.

La vista de índice actualizada se incluye en el listado 2.

**La lista 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Hay dos aspectos que debe tener en cuenta acerca de la vista actualizada en el listado 2. En primer lugar, tenga en cuenta que todo el contenido movido parte se reemplaza con una llamada a Html.RenderPartial(). Se llama al método de Html.RenderPartial() cuando se solicita la vista de índice por primera vez con el fin de mostrar el conjunto inicial de contactos.

En segundo lugar, tenga en cuenta que se ha reemplazado el Html.ActionLink() utilizado para mostrar los grupos de contactos con un Ajax.ActionLink(). Se llama a la Ajax.ActionLink() con los parámetros siguientes:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

El primer parámetro representa el texto que se mostrará para el vínculo, el segundo parámetro representa los valores de ruta y el tercer parámetro representa las opciones de Ajax. En este caso, usamos la opción de UpdateTargetId Ajax para que apunte a HTML &lt;div&gt; etiqueta que se va a actualizar una vez finalizada la solicitud de Ajax. Desea actualizar el &lt;div&gt; etiqueta con la nueva lista de contactos.

El método de Index() actualizado del controlador de contacto está contenido en el listado 3.

**El listado 3 - Controllers\ContactController.cs (método de Index)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

La acción actualizada de Index() condicionalmente devuelve uno de dos cosas. Si se invoca la acción de Index() mediante una solicitud Ajax, a continuación, el controlador devuelve un parcial. En caso contrario, la acción de Index() devuelve una vista completa.

Tenga en cuenta que la acción de Index() no es necesario devolver todos los datos cuando se invoca mediante una solicitud Ajax. En el contexto de una solicitud normal, la acción del índice devuelve una lista de todos los grupos de contactos y el grupo de contactos seleccionado. En el contexto de una solicitud Ajax, la acción de Index() devuelve solo el grupo seleccionado. AJAX significa menos trabajo en el servidor de base de datos.

La vista de índice modificada funciona en el caso de los exploradores de nivel superior y de nivel inferior. Si hace clic en un grupo de contactos y el explorador admite JavaScript, se actualiza únicamente la región de la vista que contiene la lista de contactos. Si, por otro lado, el explorador no admite JavaScript, se actualiza toda la vista.


La vista de índice actualizada tiene un problema. Al hacer clic en un grupo de contactos, no se resalta el grupo seleccionado. Dado que la lista de grupos se muestra fuera de la región que se actualiza durante una solicitud Ajax, no obtener resaltado el grupo adecuado. Este problema se corregirá en la siguiente sección.


## <a name="adding-jquery-animation-effects"></a>Agregar efectos de animación de jQuery

Normalmente, al hacer clic en un vínculo en una página web, puede usar la barra de progreso del explorador para detectar si el explorador está obteniendo activamente el contenido actualizado. Al realizar una solicitud Ajax, por otro lado, la barra de progreso de explorador no muestra ningún progreso. Esto puede hacer que los usuarios nervioso. ¿Cómo sé si el explorador ha inmovilizado?

Hay varias maneras que puede indicar a un usuario que se está realizando trabajo mientras se realiza una solicitud Ajax. Un enfoque consiste en mostrar una animación sencilla. Por ejemplo, puede hacer desaparecer una región cuando una solicitud Ajax comienza y atenuación de la región cuando se completa la solicitud.

Vamos a usar la biblioteca de jQuery que se incluye con el marco de MVC de ASP.NET de Microsoft, para crear los efectos de animación. La vista de índice actualizada se incluye en el listado 4.

**Listado 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Observe que la vista de índice actualizada contiene tres nuevas funciones de JavaScript. Las dos primeras funciones usan jQuery para atenuar y atenuación en la lista de contactos al hacer clic en un nuevo grupo de contactos. La tercera función muestra un mensaje de error cuando se produzca una solicitud de Ajax en un error (por ejemplo, tiempo de espera de red).

La primera función también se encarga de resaltado el grupo seleccionado. Una clase = atributo seleccionado se agrega al elemento primario (el elemento LI) del elemento que se hizo clic. Una vez más, jQuery facilita seleccionar el elemento correcto y agregar la clase CSS.

Estas secuencias de comandos están asociados a los vínculos de grupo con la Ayuda del parámetro Ajax.ActionLink() AjaxOptions. La llamada al método de actualizada Ajax.ActionLink() tiene el siguiente aspecto:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Agregar compatibilidad con exploradores de historial

Normalmente, al hacer clic en un vínculo para actualizar una página, se actualiza el historial del explorador. De este modo, puede hacer clic en el botón Atrás del explorador para retroceder en el tiempo a su estado anterior de la página. Por ejemplo, si hace clic en el grupo de contactos de sus amigos y, a continuación, haga clic en el grupo de contactos de negocios, hacer clic en el botón Atrás del explorador para navegar al estado de la página cuando se selecciona el grupo de contactos de sus amigos.

Por desgracia, realizar una solicitud Ajax no actualizar historial del explorador automáticamente. Si hace clic en un grupo de contactos y se recupera la lista de contactos de búsqueda de coincidencias con una solicitud Ajax, no se actualiza el historial del explorador. No se puede usar el botón Atrás del explorador para volver a un grupo de contactos después de seleccionar un nuevo grupo de contactos.

Si desea que los usuarios puedan usar el Explorador de nuevo botón después de realizar las solicitudes Ajax que necesita realizar un poco más trabajo. Necesario aprovechar la funcionalidad de administración del historial de explorador integrada en el marco de AJAX de ASP.NET.

Historial de exploración de AJAX de ASP.NET, debe hacer tres cosas:

1. Habilitar el historial de exploración estableciendo la propiedad enableBrowserHistory en true.
2. Guarde los puntos del historial cuando cambia el estado de una vista llamando al método addHistoryPoint().
3. Reconstruir el estado de la vista cuando se produce el evento navigate.

La vista de índice actualizada se incluye en el listado 5.

**Listado 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

En el listado 5, historial de exploración está habilitado en la función pageInit(). La función pageInit() también se utiliza para configurar el controlador de eventos para el evento navigate. Se produce el evento navigate cada vez que hace que el estado de la página para cambiar el explorador al día o el botón Atrás.

Se llama al método de beginContactList() al hacer clic en un grupo de contactos. Este método crea un nuevo punto del historial llamando al método addHistoryPoint(). El identificador de hacer clic en el grupo de contactos se agrega al historial.

El identificador de grupo se recupera de un atributo expando en el vínculo del grupo de contactos. El vínculo se representa con la siguiente llamada a Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

El último parámetro pasado a la Ajax.ActionLink() agrega un atributo expando denominado Id. de grupo para el vínculo (minúscula para la compatibilidad XHTML).

Cuando un usuario presiona el botón hacia delante o Atrás del explorador, se produce el evento navigate y se llama al método navigate(). Este método actualiza los contactos que se muestran en la página para que coincida con el estado de la página que se corresponde con el punto del historial de explorador pasado al método navigate.

## <a name="performing-ajax-deletes"></a>Realización de Ajax elimina

Actualmente, con el fin de eliminar un contacto, debe hacer clic en el vínculo Eliminar y, a continuación, haga clic en el botón Eliminar muestra en la página de confirmación de eliminación (consulte la figura 2). Esto parece ser una gran cantidad de solicitudes de página en algo tan simple como eliminar un registro de base de datos.


[![La página de confirmación de eliminación](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Figura 02**: la página de confirmación de eliminación ([haga clic aquí para ver la imagen a tamaño completo](iteration-7-add-ajax-functionality-cs/_static/image4.png))


Es tentador para omitir la página de confirmación de eliminación y eliminar un contacto directamente desde la vista de índice. Debe evitar la tentación porque este enfoque abre la aplicación a vulnerabilidades de seguridad. En general, don t desee realizar una operación GET de HTTP al invocar una acción que modifica el estado de la aplicación web. Al realizar una eliminación, desea realizar una solicitud HTTP POST o, mejor aún, una operación DELETE de HTTP.

El vínculo Eliminar está contenido en el ContactList parcial. Una versión actualizada de la ContactList parcial se encuentra en el listado 6.

**Listado 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

El vínculo Eliminar se representa con la siguiente llamada al método Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> El Ajax.ImageActionLink() no es una parte estándar del marco de MVC de ASP.NET. El Ajax.ImageActionLink() es un métodos de aplicación auxiliar personalizada incluidos en el proyecto póngase en contacto con el administrador.


El parámetro AjaxOptions tiene dos propiedades. En primer lugar, la propiedad confirmar se utiliza para mostrar un cuadro de diálogo de confirmación emergente JavaScript. En segundo lugar, la propiedad HttpMethod se usa para realizar una operación DELETE de HTTP.

Listado 7 contiene una nueva acción de AjaxDelete() que se ha agregado al controlador de contacto.

**Listado 7 - Controllers\ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

La acción AjaxDelete() se decora con un atributo AcceptVerbs. Este atributo se evita que la acción que invoca excepto cualquier operación de HTTP que no sea una operación DELETE de HTTP. En concreto, no se puede invocar esta acción con una solicitud HTTP GET.

Después de eliminar el registro de base de datos, debe ver una lista actualizada de contactos que no contiene el registro eliminado. El método AjaxDelete() devuelve el ContactList parcial y la lista actualizada de contactos.

## <a name="summary"></a>Resumen

En esta iteración, hemos agregado la funcionalidad de Ajax a nuestra aplicación póngase en contacto con el administrador. Se usa Ajax para mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación.

En primer lugar, hemos vuelto a diseñar la vista de índice para que al hacer clic en un grupo de contactos no se actualiza toda la vista. En su lugar, si se hace clic en un grupo de contactos, sólo se actualiza la lista de contactos.

A continuación, se usan efectos de animación de jQuery fundido de salida y atenuación en la lista de contactos. Agregar animación a una aplicación Ajax puede utilizarse para proporcionar a los usuarios de la aplicación con el equivalente de una barra de progreso del explorador.

También hemos agregado compatibilidad con exploradores de historial a nuestra aplicación de Ajax. Se habilita a los usuarios hacer clic en Atrás del explorador y reenviar botones para cambiar el estado de la vista de índice.

Por último, hemos creado un vínculo de eliminación que admite operaciones HTTP DELETE. Si realiza eliminaciones de Ajax, se permiten a los usuarios eliminar registros de base de datos sin solicitar al usuario solicitar una página de confirmación de eliminación adicionales.

>[!div class="step-by-step"]
[Anterior](iteration-6-use-test-driven-development-cs.md)
[Siguiente](iteration-1-create-the-application-vb.md)
