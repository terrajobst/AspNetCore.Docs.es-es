---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Proporciona CRUD (crear, leer, actualizar y eliminar) compatibilidad con entrada de formularios de datos | Documentos de Microsoft
author: microsoft
description: Paso 5 muestra cómo tomar nuestra clase DinnersController aún más por habilitar soporte para editar, crear y eliminar también cenas con él.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bd906282db5c620476966ffbe09cecb5ade66ee4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876753"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Proporciona CRUD (crear, leer, actualizar y eliminar) compatibilidad con entrada de formularios de datos
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 5 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 5 muestra cómo tomar nuestra clase DinnersController aún más por habilitar soporte para editar, crear y eliminar también cenas con él.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner paso 5: Crear, actualizar y eliminar los escenarios de formulario

Hemos introducido controladores y vistas y trata cómo usarlos para implementar una experiencia de lista y detalles para cenas en el sitio. El siguiente paso será seguir nuestra clase DinnersController y habilitar la compatibilidad para editar, crear y eliminar también cenas con él.

### <a name="urls-handled-by-dinnerscontroller"></a>Direcciones URL controladas por DinnersController

Se agregaron previamente los métodos de acción a DinnersController que implementa la compatibilidad con dos direcciones URL: */Dinners* y */Dinners/detalles / [id]*.

| **URL** | **VERB** | **Purpose** |
| --- | --- | --- |
| */Dinners/* | GET | Mostrar una lista HTML de cenas próximos. |
| */Dinners/Details/[id]* | GET | Mostrar los detalles sobre una cena específica. |

Ahora agregaremos los métodos de acción para implementar tres direcciones URL adicionales: <em>/Dinners/editar / [id], / cenas/Create,</em>y<em>/Dinners/Delete / [id]</em>. Estas direcciones URL habilitará la compatibilidad para edición cenas existentes, crear nuevos cenas y eliminar cenas.

Se admite interacciones de verbo HTTP GET y HTTP POST con estas nuevas direcciones URL. Las solicitudes HTTP GET para estas direcciones URL mostrará la vista HTML inicial de los datos (un formulario que se rellena con los datos de la cena en el caso de "edit", un formulario en blanco en el caso de "crear" y una pantalla de confirmación de eliminación en el caso de "delete"). Las solicitudes POST de HTTP a estas direcciones URL van a guardar, actualizar o eliminar los datos de la cena en nuestro DinnerRepository (y desde allí, a la base de datos).

| **URL** | **VERB** | **Purpose** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | GET | Mostrar un formulario HTML editable rellenado con datos de la cena. |
| EXPONER | Guarde los cambios de formato para una determinado cena a la base de datos. |
| */Dinners/Create* | GET | Mostrar un formulario HTML vacío que permite a los usuarios definir cenas nueva. |
| EXPONER | Crea una nueva cena y lo guarda en la base de datos. |
| */Dinners/Delete/[id]* | GET | Mostrar eliminación pantalla de confirmación. |
| EXPONER | Elimina la cena especificada de la base de datos. |

### <a name="edit-support"></a>Funciones de edición

Comencemos implementando el escenario "edit".

#### <a name="the-http-get-edit-action-method"></a>El método de acción de la edición de HTTP-GET

Comenzaremos por implementar el comportamiento de "GET" de HTTP de nuestro método de acción de edición. Será este método se invoca cuando el */Dinners/editar / [id]* se solicita la dirección URL. Nuestra implementación será similar:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

El código anterior usa el DinnerRepository para recuperar un objeto de la cena. A continuación, representa una plantilla de vista con el objeto de la cena. Dado que no hemos pasar explícitamente un nombre de plantilla para la *View()* método auxiliar, utilizará la ruta de acceso de manera predeterminada basada en convenciones para solucionar la plantilla de vista: /Views/Dinners/Edit.aspx.

Vamos a crear ahora esta plantilla de vista. Para hacer esto, haga clic dentro del método de editar y seleccione el comando de menú "Agregar vista" contexto:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

En el cuadro de diálogo "Agregar vista" se deberá indicar que se está pasando un objeto de la cena a la plantilla de vista como su modelo de y elegir scaffold automáticamente una plantilla de "Editar":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Cuando se hace clic en el botón "Agregar", Visual Studio agregará un nuevo archivo de plantilla de la vista "Edit.aspx" para que podamos dentro del directorio "\Views\Dinners". También se abrirá la nueva plantilla de vista de "Edit.aspx" en el editor de código: rellenado con una "Editar" scaffold implementación inicial como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Vamos a realizar algunos cambios en el scaffold "Editar" predeterminado que se genera y actualizar la plantilla de vista de edición para que el contenido siguiente (lo que elimina algunas de las propiedades que no desea exponer):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Cuando se ejecutan la aplicación y la solicitud del *"/ cenas/editar/1"* dirección URL se mostrará la página siguiente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

El formato HTML generado por la vista es similar a continuación. Es HTML estándar: con un &lt;formulario&gt; elemento que realiza una solicitud HTTP POST a la */Dinners/Edit/1* dirección URL cuando "Guardar" &lt;tipo de entrada = "Enviar" /&gt; botón se inserta. Una HTML &lt;tipo de entrada = "text" /&gt; elemento ha sido salida para cada propiedad editable:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() y los métodos de aplicación auxiliar Html Html.TextBox()

La plantilla de vista "Edit.aspx" está utilizando varios métodos de "aplicación auxiliar de Html": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() y Html.ValidationMessage(). Además de generar el marcado HTML para que podamos, estos métodos auxiliares proporcionan control de errores integrada y validación admiten.

##### <a name="htmlbeginform-helper-method"></a>Método de aplicación auxiliar de Html.BeginForm()

El método de aplicación auxiliar de Html.BeginForm() es lo que el código HTML de salida &lt;formulario&gt; elemento en el marcado. En la plantilla de vista Edit.aspx observará que se aplica una instrucción "using" cuando se usa este método de C#. La llave de apertura indica el principio de la &lt;formulario&gt; contenido y la llave de cierre es lo que indica el final de la &lt;/formar&gt; elemento:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

O bien, si se encuentra la instrucción "using" enfoque resultaría poco natural para un escenario similar al siguiente, puede usar una combinación de Html.BeginForm() y Html.EndForm() (que hace lo mismo):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Al llamar a Html.BeginForm() sin ningún parámetro, provocará que se generar un elemento de formulario que realiza una solicitud HTTP POST a la dirección URL de la solicitud actual. Es decir ¿por qué la vista de edición genera un *&lt;acción de formulario = "/ cenas/editar/1" método = "post"&gt;* elemento. Podríamos haber también pasamos parámetros explícitos a Html.BeginForm() si quería volver a publicar en una dirección URL diferente.

##### <a name="htmltextbox-helper-method"></a>Método de aplicación auxiliar de Html.TextBox()

Nuestro punto de vista Edit.aspx usa el método de aplicación auxiliar de Html.TextBox() para salida &lt;tipo de entrada = "text" /&gt; elementos:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

El método de Html.TextBox() anterior toma un único parámetro, que se usa para especificar los atributos de identificador/nombre de la &lt;tipo de entrada = "text" /&gt; elemento a la salida, así como la propiedad de modelo para rellenar el valor del cuadro de texto de. Por ejemplo, el objeto de la cena se pasa a la vista de edición tenía un valor de propiedad de "Título" de ".NET futuros" y lo que el método Html.TextBox("Title") llamadas de salida: *&lt;Id. de entrada = "Title" name = "Title" type = "text" value = ".NET futuros" /&gt;*.

Como alternativa, podemos utilizar el primer parámetro de Html.TextBox() para especificar el identificador/nombre del elemento y, a continuación, pasar de manera explícita en el valor que se usará como un segundo parámetro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

A menudo es conveniente trabajar con un formato personalizado en el valor que se envía. El método estático String.Format () integrada en .NET es útil para estos escenarios. Está usando para dar formato al valor de Fecha delevento (que es de tipo DateTime) de la plantilla de vista Edit.aspx para que no muestra a segundos para el tiempo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un tercer parámetro Html.TextBox() opcionalmente puede usarse para generar los atributos HTML adicionales. El fragmento de código de siguiente muestra cómo representar un tamaño adicional = atributo "30" y una clase = "mycssclass" atributo en el &lt;tipo de entrada = "text" /&gt; elemento. Tenga en cuenta cómo se estamos secuencias de escape el nombre de la clase de atributo utilizando una "@" character because "clase" es una palabra reservada en C#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementar el método de acción de edición de HTTP-POST

Ahora tenemos la versión de HTTP-GET de nuestro método de acción de editar implementado. Cuando un usuario solicita la */Dinners/Edit/1* dirección URL que reciben una página HTML similar al siguiente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Al presionar el botón "Guardar" hace que un formulario post a la */Dinners/Edit/1* de dirección URL, y envía el código HTML &lt;entrada&gt; valores mediante el verbo HTTP POST de formulario. Implementemos ahora el comportamiento de HTTP POST de nuestro método de acción de edición: que se encargará de guardar la cena.

Comenzaremos agregando un método de acción "Editar" sobrecargado a nuestro DinnersController que tiene un atributo de "AcceptVerbs" en lo que indica que controla los escenarios de HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Cuando el atributo [AcceptVerbs] se aplica a los métodos de acción sobrecargado, ASP.NET MVC controla automáticamente las solicitudes de distribución para el método de acción adecuado según el verbo HTTP entrante. Las solicitudes HTTP POST <em>/Dinners/editar / [id]</em> direcciones URL pasará al método edición anterior, mientras todas las demás solicitudes de verbo HTTP para <em>/Dinners/editar / [id]</em>pasa las direcciones URL para el primer método de edición se implementa (que se ha no tiene un atributo [AcceptVerbs]).

| **Tema de lado: ¿Por qué diferenciar a través de verbos HTTP?** |
| --- |
| Puede que se pregunte: ¿por qué se usa una sola dirección URL y diferenciar su comportamiento mediante el verbo HTTP? ¿Por qué no solo tiene dos direcciones URL independientes para controlar cargar y guardar los cambios de edición? ¿Por ejemplo: /Dinners/editar / [id] para mostrar el formulario inicial y /Dinners/guardar / [id] para controlar el envío de formulario para guardarlo? El inconveniente con publicación dos URL independientes es que, en casos donde enviar a /Dinners/Save/2 y, a continuación, debe volver a mostrar el formulario HTML debido a un error de entrada, el usuario final se acaban por tener la dirección URL de cenas/guardar/2 en la barra de direcciones de su explorador (ya que ese era la dirección URL del formulario envía al). Si el usuario final coloca una marca en esta página redisplayed a su lista de favoritos del explorador, o copia y pega la dirección URL y envía por correo electrónico a un amigo, terminará guardar una dirección URL que no funcionará en el futuro (ya que esa dirección URL depende de los valores de entrada). Mediante la exposición de una sola dirección URL (como: /Dinners/Edit/[id]) y diferenciar el procesamiento con el verbo HTTP, es seguro para los usuarios finales a la página de edición de marcador o envíe la dirección URL a otras personas. |

#### <a name="retrieving-form-post-values"></a>Recuperar valores de entrada de formulario

Hay varias maneras podemos acceder a registrado parámetros de formulario en el método de "Editar" HTTP POST. Una estrategia sencilla consiste en que se puede utilizar la propiedad de solicitud en la clase base del controlador para tener acceso a la colección de formulario y recuperar los valores expuestos directamente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Sin embargo, el enfoque anterior es un poco verbose, especialmente cuando se agregue lógica de control de errores.

Un enfoque más satisfactorio para este escenario consiste en aprovechar integrado *UpdateModel()* método auxiliar en la clase base del controlador. Admite la actualización de las propiedades de un objeto que se pasa con los parámetros de formulario entrante. Utiliza la reflexión para determinar los nombres de propiedad en el objeto y, a continuación, automáticamente convierte y asigna valores a ellos en función de los valores de entrada enviados por el cliente.

Podríamos usar el método UpdateModel() para simplificar nuestra HTTP-POST Editar acción con este código:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Ahora podemos visitemos el */Dinners/Edit/1* dirección URL y el título de la cena de cambios:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Cuando se hace clic en el botón "Guardar", se llevarán a cabo un formulario post a la acción de editar y los valores actualizados se conservarán en la base de datos. Hemos, a continuación, se redirigirán a la dirección URL de detalles de la cena (que mostrará los valores guardados recién):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Controlar los errores de edición

Nuestro actual funciona de implementación de HTTP-POST correctamente: excepto cuando hay errores.

Cuando un usuario realiza un error en la edición de un formulario, se tiene que asegurarse de que se vuelve a mostrar el formulario con un mensaje de error informativo que les sirve de guía para corregirlo. Esto incluye los casos donde un usuario final envía una entrada incorrecta (por ejemplo: una cadena de fecha con formato incorrecto), tal y como además de casos donde el formato de entrada es válido pero no hay una infracción de regla de negocio. Cuando se producen errores que el formulario debe conservar los datos de entrada especificado originalmente para que no tengan que rellenar sus cambios manualmente por el usuario. Debe repetir este proceso tantas veces como sea necesario hasta que el formulario se complete correctamente.

ASP.NET MVC incluye algunas características integradas como "nice" que facilitan el control de errores y volver a mostrar del formulario. Para ver estas características en acción vamos a actualizar el método de acción de edición con el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

El código anterior es similar a nuestra implementación anterior, salvo que ahora nos estamos ajuste un bloque de control de errores try/catch alrededor de nuestro trabajo. Si se produce una excepción al llamar a UpdateModel() o cuando se intentan y guarda la DinnerRepository (que se producirá una excepción si el objeto de la cena que estamos intentando guardar no es válido debido a una infracción de regla dentro de nuestro modelo), el bloque de control de error catch realizará las siguientes ejecutar. En él se recorrer las infracciones de reglas que existen en el objeto de la cena y agregarlos a un objeto ModelState (que trataremos en breve). A continuación, se vuelva a mostrar la vista.

Para ver esto permite volver a ejecutar la aplicación en funcionamiento, edite una cena y cámbielo para tener un título vacío, una fecha delevento de "BOGUS" y usar un número de teléfono del Reino Unido con un valor de país de Estados Unidos. Cuando se presiona el botón "Guardar" el método HTTP POST editar no podrá guardar la cena (porque no hay errores) y se volverá a mostrar el formulario:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Nuestra aplicación tiene una experiencia de error decente. Los elementos de texto con la entrada no válida se resaltan en rojo y se muestran mensajes de error de validación para el usuario final acerca de ellos. El formulario también conserva los datos de entrada que se especificó originalmente el usuario: para que no tengan que recargar nada.

¿Cómo, podría preguntar, esto sucedió? ¿Cómo los cuadros de texto de título, Fecha delevento y Teléfonodecontacto por sí mismos resaltar en rojo y saber para generar los valores de usuario especificada originalmente? ¿Y cómo se muestren mensajes de error en la lista en la parte superior? Las buenas noticias son que esto no se produce por arte de magia; en su lugar, era porque se utilizan algunas de las características de ASP.NET MVC integradas que facilitan la validación de entrada y de error escenarios de control.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Descripción ModelState y los métodos de aplicación auxiliar HTML de validación

Las clases de controlador tienen una colección de propiedades de "ModelState" que proporciona una manera de indicar la existen de errores con un objeto de modelo que se pasa a una vista. Entradas de error dentro de la colección ModelState identifican el nombre de la propiedad de modelo con el problema (por ejemplo: "Title", "Fecha delevento" o "Teléfonodecontacto") y permiten a un mensaje de error humanos sencillo que se especifique (por ejemplo: "El título es obligatorio").

El *UpdateModel()* método auxiliar que rellena automáticamente la colección de ModelState cuando encuentra errores al intentar asignar valores de formulario a propiedades en el objeto de modelo. Por ejemplo, la propiedad de Fecha delevento del objeto de la cena es de tipo DateTime. Cuando el método UpdateModel() no pudo asignar el valor de cadena "BOGUS" a él en el escenario anterior, el método UpdateModel() agrega una entrada a la colección de ModelState que indica un error de asignación se hubiera producido con esa propiedad.

Los desarrolladores también pueden escribir código para agregar entradas de error en la colección ModelState de forma explícita como que hacemos a continuación en el bloque de control de error de "detectar", que se rellena la colección ModelState con entradas en función de las infracciones de reglas activo en el Objeto de la cena:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integración de la aplicación auxiliar HTML con ModelState

Métodos de aplicación auxiliar HTML - como Html.TextBox() - comprueban la colección de ModelState al representar los resultados. Si existe un error para el elemento, se presentan el valor escrito por el usuario y una clase de errores CSS.

Por ejemplo, en la vista "Editar" Estamos utilizando el método de aplicación auxiliar Html.TextBox() para representar la fecha delevento de nuestro objeto cena:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Cuando se represente la vista en el escenario de error, el método Html.TextBox() comprueba la colección ModelState para ver si hay algún error asociado a la propiedad "fecha delevento" de nuestro objeto cena. Cuando determina que se produjo un error representa la entrada de usuario enviado ("BOGUS") como el valor y agrega una clase de errores de css para el &lt;tipo de entrada = "cuadro de texto" /&gt; marcado que genera:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Puede personalizar la apariencia de la clase de error de css para buscar que desee. La clase de error CSS predeterminado: "entrada-error de validación": se define en el *\content\site.css* hoja de estilos y es similar a continuación:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Esta regla CSS es la causa de nuestros elementos de entrada no válidos se van a resaltar como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Método de aplicación auxiliar de Html.ValidationMessage()

El método de aplicación auxiliar de Html.ValidationMessage() puede utilizarse para generar el mensaje de error ModelState asociado a una propiedad de modelo determinado:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

El código anterior genera:  *&lt;span (clase) = "error de validación de campo"&gt; no es válido el valor 'BOGUS' &lt; /span&gt;*

El método de aplicación auxiliar de Html.ValidationMessage() también admite un segundo parámetro que permite a los desarrolladores invalidar el mensaje de texto de error que se muestra:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

El código anterior genera:  <em>&lt;span (clase) = "error de validación de campo"&gt;\*&lt;/abarcan&gt;</em>en lugar del texto de error de forma predeterminada, cuando hay un error para el Propiedad de Fecha delevento.

##### <a name="htmlvalidationsummary-helper-method"></a>Método de aplicación auxiliar de Html.ValidationSummary()

El método de aplicación auxiliar de Html.ValidationSummary() puede usarse para representar un mensaje de error de resumen, acompañado de un &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; lista de error detallado de todos los mensajes en la Colección de ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

El método de aplicación auxiliar de Html.ValidationSummary() toma un parámetro de cadena opcional, que define un mensaje de error de resumen para mostrar encima de la lista de errores detallados:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

También puede usar CSS para invalidar el aspecto de la lista de errores.

#### <a name="using-a-addruleviolations-helper-method"></a>Mediante un método auxiliar AddRuleViolations

Nuestra implementación inicial de editar HTTP-POST utiliza una instrucción foreach dentro de su bloque catch para recorrer las infracciones de reglas del objeto cena y agregarlas a la colección de ModelState del controlador:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Podemos hacer que este código de clase al proyecto NerdDinner un poco limpiador agregando un "ControllerHelpers" e implementar un método de extensión "AddRuleViolations" dentro de él que agrega un método auxiliar a la clase de ASP.NET MVC ModelStateDictionary. Este método de extensión puede encapsular la lógica necesaria para rellenar el ModelStateDictionary con una lista de errores de RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

A continuación, podemos actualizar el método de acción HTTP-POST Editar para usar este método de extensión para rellenar la colección ModelState con nuestro infracciones de reglas de la cena.

#### <a name="complete-edit-action-method-implementations"></a>Completar las implementaciones de método de acción de edición

El código siguiente implementa toda la lógica de controlador necesaria para el escenario de edición:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Los aspectos positivos de nuestra implementación de edición es que nuestra clase de controlador ni la plantilla de vista tiene que saber nada acerca de la validación específica o las reglas de negocios que se aplica al nuestro modelo cena. Se pueden agregar más reglas a nuestro modelo en el futuro y no es necesario realizar cambios de código a nuestro controlador o vista en orden para que puedan ser compatibles. Esto nos proporciona la flexibilidad necesaria para desarrollar fácilmente nuestros requisitos de la aplicación en el futuro con un mínimo de cambios en el código.

### <a name="create-support"></a>Crear soporte técnico

Hemos terminado de implementar el comportamiento de "Edit" de nuestra clase DinnersController. Ahora pasemos a implementar la compatibilidad de "Crear" en él, lo que le permitirá a los usuarios agregar cenas nueva.

#### <a name="the-http-get-create-action-method"></a>HTTP-GET crear método de acción

Comenzaremos por implementar el comportamiento de "GET" de HTTP de nuestro crear método de acción. Este método se llamará cuando un usuario visita el */cenas/crear* dirección URL. Nuestra implementación tiene el siguiente aspecto:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

El código anterior crea un nuevo objeto de cena y asigna la propiedad de Fecha delevento sea una semana en el futuro. A continuación, representa una vista que se basa en el nuevo objeto de la cena. Dado que no hemos pasar explícitamente un nombre para el *View()* método auxiliar, utilizará la ruta de acceso de manera predeterminada basada en convenciones para solucionar la plantilla de vista: /Views/Dinners/Create.aspx.

Vamos a crear ahora esta plantilla de vista. Podemos hacerlo con el botón secundario en el método de acción Create y seleccionando el comando de menú contextual "Agregar vista". En el cuadro de diálogo "Agregar vista" se deberá indicar que se está pasando un objeto de la cena a la plantilla de vista y decide scaffold automáticamente una plantilla de "Crear":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Cuando se hace clic en el botón "Agregar", Visual Studio guardar una nueva vista de "Create.aspx" basada en scaffolding en el directorio "\Views\Dinners" y abra el IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Vamos a realizar algunos cambios al "crear" scaffold archivo predeterminado que se generó automáticamente y modificar seguridad para que sea similar a continuación:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Y ahora cuando se ejecución la aplicación y el acceso del *"/ cenas/crear"* dirección URL en el explorador se representan en nuestra implementación de la acción de crear interfaz de usuario como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementar el HTTP-POST crear método de acción

Contamos con la versión de HTTP-GET de nuestro método de acción de crear implementado. Cuando un usuario hace clic en el botón "Guardar" realiza una solicitud post de formulario a la */cenas/crear* de dirección URL, y envía el código HTML &lt;entrada&gt; valores mediante el verbo HTTP POST de formulario.

Implementemos ahora el comportamiento de HTTP POST de nuestro crear método de acción. Comenzaremos agregando un método de acción "Crear" sobrecargado a nuestro DinnersController que tiene un atributo de "AcceptVerbs" en lo que indica que controla los escenarios de HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Hay varias maneras podemos acceder a los parámetros de formulario expuesto dentro de nuestro método "Crear" de HTTP-POST habilitado.

Un enfoque consiste en crear un nuevo objeto de cena y, a continuación, use la *UpdateModel()* método de aplicación auxiliar (al igual que hicimos con la acción de edición) para rellenar con los valores de formulario expuesto. A continuación, podemos agregar a nuestra DinnerRepository, almacenar los datos en la base de datos y redirigir al usuario a la acción de detalles para mostrar de la cena recién creada con el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Como alternativa, podemos usar un enfoque donde tenemos nuestro método de acción Create() toman un objeto de la cena como un parámetro de método. ASP.NET MVC se, a continuación, automáticamente instancias de un nuevo objeto de cena para que podamos, rellenar sus propiedades en las entradas de formulario y pasar a nuestro método de acción:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

El método de acción anterior comprueba que el objeto de la cena se haya rellenado correctamente con los valores de entrada de formulario mediante la comprobación de la propiedad ModelState.IsValid. Esto devolverá false si no hay problemas de conversión de entrada (por ejemplo: una cadena de "BOGUS" para la propiedad de Fecha delevento), y si hay algún problema el método de acción vuelve a mostrar el formulario.

Si los valores de entrada son válidos, a continuación, el método de acción intenta agregar y guardar la cena nuevo en el DinnerRepository. Ajusta este trabajo dentro de un bloque try/catch y vuelve a mostrar el formulario si hay alguna infracción de regla de negocios (lo que haría que el método dinnerRepository.Save() generar una excepción).

Para ver este comportamiento en acción de control de errores, se puede solicitar la */cenas/crear* dirección URL y rellene los detalles acerca de la cena de nuevo. Entrada incorrecta o valores hará que el formulario de creación se vuelve a mostrar con los errores resaltados como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Tenga en cuenta cómo nuestro formulario de creación está respetando las reglas de validación y business exactas mismo como nuestro formulario de edición. Esto es porque las validación y reglas de negocios se definieron en el modelo y no se incrustan dentro de la interfaz de usuario o el controlador de la aplicación. Esto significa podemos más adelante cambio/evolucionar la validación o las reglas de negocios en un único equipo coloque y que se aplique a lo largo de nuestra aplicación. No tenemos que cambiar ningún código dentro de cualquiera nuestro editar o crear métodos de acción para respetar automáticamente las nuevas reglas o modificaciones a las ya existentes.

Cuando se corrija los valores de entrada y haga clic en el botón "Guardar", nuestra adición a la DinnerRepository se realizará correctamente y una cena nueva se agregará a la base de datos. A continuación, se redirigirá a la */Dinners/detalles / [id]* dirección URL: donde se le presenta detalles acerca de la cena recién creada:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Eliminar el soporte técnico

Agreguemos ahora "Delete" soporte a nuestro DinnersController.

#### <a name="the-http-get-delete-action-method"></a>El método de acción Delete de HTTP-GET

Comenzaremos por implementar el comportamiento de HTTP GET de nuestro método de acción delete. Se llamará a este método cuando un usuario visita el */Dinners/Delete / [id]* dirección URL. A continuación se muestra la implementación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

El método de acción intenta recuperar la cena va a eliminar. Si la cena existe representa una vista se basa en el objeto de la cena. Si el objeto no existe (o ya ha sido eliminado) se devuelve una vista que representa "NotFound" ver plantilla que se creó anteriormente para el método de acción "Detalles".

Podemos crear la plantilla de la vista "Delete" con el botón secundario dentro del método de acción de eliminación y seleccionando el comando de menú contextual "Agregar vista". En el cuadro de diálogo "Agregar vista" se deberá indicar que se está pasando un objeto de la cena a nuestra plantilla vista como su modelo y optar por crear una plantilla vacía:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Cuando se hace clic en el botón "Agregar", Visual Studio agregará un nuevo archivo de plantilla de la vista "Delete.aspx" para que podamos dentro de nuestro directorio "\Views\Dinners". Vamos a agregar algún HTML y el código a la plantilla para implementar una pantalla de confirmación de eliminación como las siguientes:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

El código anterior muestra el título de la cena va a eliminar y las salidas una &lt;formulario&gt; elemento que realiza una operación POST a la URL de /Dinners/Delete / [id] si el usuario final hace clic en el botón "Eliminar" dentro de él.

Cuando se ejecutan la aplicación y el acceso del *"/ cenas/Delete / [id]"* objeto de dirección URL para una cena válida que se representa como interfaz de usuario a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Tema de lado: ¿Por qué lo hacemos una entrada de blog?** |
| --- |
| Puede que se pregunte: ¿por qué decidimos implementar a través de la intervención de creación de un &lt;formulario&gt; dentro de la pantalla de confirmación de eliminación? ¿Por qué no puede utilizar un hipervínculo estándar para vincular a un método de acción que realiza la operación de eliminación real? La razón es que queremos tener cuidado y protegerse de rastreadores de web y detectar nuestras direcciones URL de archivo y provoquen accidentalmente datos se eliminarán cuando siguen los vínculos de motores de búsqueda. HTTP-GET según las direcciones URL se consideran "seguros" para que access/rastreo y se supone que no se siguen los HTTP-POST. Una regla útil consiste en asegurarse de que se ha de colocar siempre destructiva u operaciones de modificación de datos detrás de las solicitudes HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementar el método de acción Delete de HTTP-POST

Ahora tenemos la versión de HTTP-GET de nuestro método de acción Delete implementado que muestra una pantalla de confirmación de eliminación. Cuando un usuario final hace clic en el botón "Eliminar" llevará a cabo una solicitud post de formulario a la */Dinners/Dinner / [id]* dirección URL.

Implementemos ahora el comportamiento HTTP "POST" del método de acción de eliminación con el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La versión de HTTP-POST de nuestro método de acción de eliminación intenta recuperar el objeto de la cena que se va a eliminar. Si no lo encuentra (porque ya se ha eliminado) representa la plantilla de "NotFound". Si se encuentra la cena, elimina de la DinnerRepository. A continuación, representa una plantilla de "Eliminado".

Para implementar la plantilla de "Eliminado" Comenzaremos pulse el botón derecho en el método de acción y elija el menú contextual de "Agregar vista". Comenzaremos mencione la vista "Eliminado" y tiene ser una plantilla vacía (y no toman un objeto de modelo fuertemente tipado). A continuación, vamos a agregar contenido HTML a él:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Y ahora cuando se ejecución la aplicación y el acceso del *"/ cenas/Delete / [id]"* dirección URL para una cena válida confirmación para eliminar el objeto representará nuestra cena pantalla como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Cuando se hace clic en el botón "Eliminar" llevará a cabo un solicitud HTTP POST a la */Dinners/Delete / [id]* dirección URL, que se elimine de la cena de nuestra base de datos y mostrará la plantilla de vista "Eliminado":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Seguridad del modelo de enlace

Hemos hablado de dos maneras diferentes de utilizar las características integradas de enlace del modelo de ASP.NET MVC. El primero mediante el método UpdateModel() para actualizar las propiedades en un objeto de modelo existente y el segundo con soporte técnico de ASP.NET MVC para pasar los objetos del modelo en como parámetros de método de acción. De estas técnicas son muy eficaces y muy útil.

Esta eficacia también aporta responsabilidad. Es importante para que siempre esté paranoicos acerca de la seguridad al aceptar la intervención del usuario y lo mismo sucede al enlazar objetos a la entrada del formulario. Debe tener cuidado de HTML codificar siempre los valores introducidos por el usuario para evitar ataques de inyección de código HTML y JavaScript y tenga cuidado de ataques de inyección de SQL (Nota: estamos utilizando LINQ to SQL para nuestra aplicación, que codifica automáticamente los parámetros para evitar estos tipos de ataques). Nunca debe confiar en la validación del lado cliente independiente y siempre emplean la validación del lado servidor para protegerse contra piratas informáticos que intentan enviar valores ficticias.

Un elemento de seguridad adicional para asegurarse de que pensar al usar las características de enlace de ASP.NET MVC es el ámbito de los objetos que se va a enlazar. En concreto, desea asegurarse de que comprenda las implicaciones de seguridad de las propiedades que se va a permitir se puede enlazar y asegúrese de que solo admite las propiedades que realmente deben poder actualizables un usuario final para actualizarse.

De forma predeterminada, el método UpdateModel() intentará actualizar todas las propiedades en el objeto de modelo que coinciden con los valores de parámetro de formulario entrante. Del mismo modo, pueden tener objetos que se pasan como parámetros de método de acción también de forma predeterminada todas sus propiedades establecidas a través de los parámetros del formulario.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Bloqueo de enlace de forma por uso

Puede bloquear la directiva de enlace según una por cada uso en proporcionar una explícita "incluir lista" de propiedades que se pueden actualizar. Para ello puede pasar un parámetro de matriz de cadena adicional al método UpdateModel() como a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objetos que se pasan como parámetros de método de acción también admiten un atributo [Bind] que permite un "incluyen lista" permitido propiedades que se especifique como a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bloqueo de enlace según el tipo

También puede bloquear las reglas de enlace por tipo. Esto le permite especificar las reglas de enlace una vez y, a continuación, que se les aplique en todos los escenarios (incluidos los escenarios de parámetro de método UpdateModel y acción) en todos los controladores y métodos de acción.

Puede personalizar las reglas de enlace por tipo mediante la adición de un atributo [Bind] en un tipo, o si se registra en el archivo Global.asax de la aplicación (es útil para escenarios donde no tiene el tipo). A continuación, puede utilizar Include el enlace del atributo y Exclude propiedades para controlar qué propiedades son enlazables para la interfaz o clase determinada.

Se podrá usar esta técnica para la clase de la cena de nuestra aplicación NerdDinner y agregue un atributo [Bind] que restringe la lista de propiedades se puede enlazar a lo siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Tenga en cuenta no permite que la colección de confirmaciones de asistencia para su manipulación a través del enlace, así como tampoco se permite que las propiedades DinnerID o HostedBy a establecerse a través del enlace. Por motivos de seguridad en su lugar, se podrá manipular solo estas propiedades determinadas mediante código explícito en nuestros métodos de acción.

### <a name="crud-wrap-up"></a>Resumen CRUD

ASP.NET MVC incluye una serie de características integradas que ayudan a implementar escenarios de envío de formulario. Se usa una variedad de estas características para proporcionar compatibilidad de interfaz de usuario de CRUD sobre nuestra DinnerRepository.

Estamos usando un enfoque centrada en el modelo para implementar la aplicación. Esto significa que todos nuestros validación y regla de negocios lógica se define en nuestro nivel de modelo: y no en nuestro controladores o vistas. Nuestra clase de controlador ni nuestras plantillas de vista sepa nada acerca de las reglas de negocios específicas que se aplica al nuestra clase de modelo de la cena.

Esto mantendrá nuestra arquitectura de aplicación correcto y que resulten más fáciles de probar. Podemos agregar reglas de negocio adicionales a la capa de modelo en el futuro y *no tiene que realizar ningún cambio de código* a nuestro controlador o vista en orden para ellos que se deben admitir. Esto se va a proporcionar un gran agilidad evolucionando y cambiar la aplicación en el futuro.

Nuestro DinnersController ahora permite cena listas o detalles, así como crear, editar y eliminar soporte técnico. El código completo de la clase puede encontrarse a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Paso siguiente

Ahora tenemos la compatibilidad básica de CRUD (crear, leer, actualizar y eliminar) implementar dentro de nuestra clase DinnersController.

Ahora veamos cómo podemos usar clases ViewData y ViewModel para habilitar incluso más rica interfaz de usuario en los formularios.

> [!div class="step-by-step"]
> [Anterior](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Siguiente](use-viewdata-and-implement-viewmodel-classes.md)
