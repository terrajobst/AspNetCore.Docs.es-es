---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Proporcionar CRUD (crear, leer, actualizar y eliminar) compatibilidad con entradas de formularios de datos | Microsoft Docs
author: microsoft
description: Paso 5 muestra cómo aprovechar nuestra clase DinnersController aún más al habilitar la compatibilidad con para editar, crear y eliminar instancias Dinners con ella también.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 821684c0753967fc1a693b061d5d539951cd7c23
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388813"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Proporcionar CRUD (crear, leer, actualizar y eliminar) compatibilidad con entradas de formularios de datos
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 5 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 5 muestra cómo aprovechar nuestra clase DinnersController aún más al habilitar la compatibilidad con para editar, crear y eliminar instancias Dinners con ella también.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner paso 5: Crear, actualizar y eliminar los escenarios de formulario

Hemos introducido controladores y vistas y ha explicado cómo usarlas para implementar una experiencia de lista/detalles para las cenas en el sitio. El siguiente paso será ir más allá de nuestra clase DinnersController y habilitar la compatibilidad de edición, crear y eliminar también las cenas con él.

### <a name="urls-handled-by-dinnerscontroller"></a>Direcciones URL que se controlan mediante DinnersController

Se agregaron previamente los métodos de acción a DinnersController que implementa la compatibilidad con dos direcciones URL: */dinners* y */dinners/detalles / [id]*.

| **URL** | **VERBO** | **Propósito** |
| --- | --- | --- |
| */Dinners/* | GET | Mostrar una lista HTML de instancias dinners próximas. |
| */ Dinners/detalles / [id]* | GET | Mostrar los detalles sobre una cena específico. |

Ahora agregaremos métodos de acción para implementar tres direcciones URL adicionales: <em>/dinners/Edit / [id], / Dinners/Create,</em>y<em>/dinners/Delete / [id]</em>. Estas direcciones URL permitirá la compatibilidad con la edición Dinners existentes, crear instancias Dinners nuevas y eliminar las cenas.

Admitimos interacciones de verbo HTTP GET y HTTP POST con estas nuevas direcciones URL. Las solicitudes HTTP GET a estas direcciones URL mostrará la vista HTML inicial de los datos (un formulario que se rellena con los datos de la cena en el caso de "Editar", un formulario en blanco en el caso de "crear" y una pantalla de confirmación de eliminación en el caso de "delete"). Las solicitudes HTTP POST a estas direcciones URL va a guardar, actualizar o eliminar los datos de la cena en nuestra instancia DinnerRepository (y desde allí, a la base de datos).

| **URL** | **VERBO** | **Propósito** |
| --- | --- | --- |
| */ Dinners/edit / [id]* | GET | Mostrar un formulario HTML modificable que se rellena con datos de la cena. |
| EXPONER | Guarde los cambios del formulario para una cena en particular a la base de datos. |
| */ Dinners/crear* | GET | Mostrar un formulario HTML vacío que permite a los usuarios definir instancias Dinners nuevo. |
| EXPONER | Crea una cena nuevo y lo guarda en la base de datos. |
| */ Dinners/delete / [id]* | GET | Mostrar eliminación pantalla de confirmación. |
| EXPONER | Elimina la cena especificada de la base de datos. |

### <a name="edit-support"></a>Editar el soporte técnico

Comencemos implementando el escenario "Editar".

#### <a name="the-http-get-edit-action-method"></a>El método de acción Edit de HTTP-GET

Comenzaremos por implementar el comportamiento HTTP "GET" de nuestro método de acción de edición. Será este método se invoca cuando el */dinners/Edit / [id]* se solicita la dirección URL. Nuestra implementación tendrá un aspecto como:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

El código anterior usa la instancia DinnerRepository para recuperar un objeto de la cena. A continuación, representa una plantilla de vista mediante el objeto de la cena. Dado que no hemos pasado explícitamente un nombre de plantilla para el *View()* método auxiliar, utilizará la ruta de acceso de forma predeterminada basada en convenciones para resolver la plantilla de vista: /Views/Dinners/Edit.aspx.

Vamos a crear ahora esta plantilla de vista. Para hacer esto con el botón secundario dentro del método de edición y seleccionando el comando del menú contextual "Agregar vista":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

En el cuadro de diálogo "Agregar vista" indicaremos que se está pasando un objeto de la cena a nuestra plantilla de vista como su modelo y elegir una plantilla de "Editar" automática scaffolding:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio agregará un nuevo archivo de plantilla de vista "Edit.aspx" para nosotros dentro del directorio "\Views\Dinners". También se abrirá la nueva plantilla de vista "Edit.aspx" en el editor de código: rellenado con una "Editar" scaffold implementación inicial, como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Vamos a realizar algunos cambios en el scaffold "Editar" predeterminado generada y actualice la plantilla de vista de edición para que el contenido siguiente (lo que elimina algunas de las propiedades que no deseamos exponer):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Cuando se ejecutan la aplicación y la solicitud la *"/ Dinners/Edit/1"* dirección URL que se mostrará la página siguiente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

El formato HTML generado por nuestra vista como el siguiente. Es HTML estándar: con un &lt;formulario&gt; elemento que realiza una solicitud HTTP POST a la */Dinners/Edit/1* dirección URL al "Guardar" &lt;tipo de entrada = "submit" /&gt; botón está presionado. Una HTML &lt;tipo de entrada = "text" /&gt; elemento ha sido de salida para cada propiedad modificable:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() y los métodos de aplicación auxiliar Html Html.TextBox()

Nuestra plantilla de vista "Edit.aspx" está utilizando varios métodos de "Aplicación auxiliar Html": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() y Html.ValidationMessage(). Además de generar el marcado HTML para nosotros, estos métodos auxiliares proporcionan control de errores integrada y validación de soporte técnico.

##### <a name="htmlbeginform-helper-method"></a>Método auxiliar de Html.BeginForm()

El método auxiliar Html.BeginForm() es lo que el código HTML de salida &lt;formulario&gt; elemento nuestro marcado. En la plantilla de vista Edit.aspx observará que estamos aplicando una instrucción "using" al usar este método de C#. La llave de apertura indica el principio de la &lt;formulario&gt; contenido y la llave de cierre es lo que indica el final de la &lt;/formar&gt; elemento:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Como alternativa, si se encuentra la instrucción "using" enfoque poco natural para un escenario similar al siguiente, puede usar una combinación de Html.BeginForm() y Html.EndForm() (lo que hace lo mismo):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Una llamada a Html.BeginForm() sin ningún parámetro dará lugar a la salida de un elemento de formulario que realiza una solicitud HTTP POST a la dirección URL de la solicitud actual. Es decir por qué nuestra vista de edición genera un *&lt;acción de formulario = "/ Dinners/Edit/1" método = "post"&gt;* elemento. Podríamos haber también pasamos parámetros explícitos a Html.BeginForm() si deseáramos publicar en una dirección URL diferente.

##### <a name="htmltextbox-helper-method"></a>Método auxiliar de Html.TextBox()

Nuestra vista Edit.aspx usa el método auxiliar Html.TextBox() para generar &lt;tipo de entrada = "text" /&gt; elementos:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

El método Html.TextBox() anterior toma un único parámetro, que se usa para especificar los atributos de identificador/nombre de la &lt;tipo de entrada = "text" /&gt; elemento a la salida, así como para rellenar el valor del cuadro de texto de la propiedad del modelo. Por ejemplo, el objeto de Dinner que se pasó a la vista de edición tenía un valor de propiedad "Title" de ".NET futuros" y lo que nuestro método Html.TextBox("Title") llamadas de salida: *&lt;Id. de entrada = "Title" name = "Title" type = "text" value = ".NET Futures" /&gt;*.

Como alternativa, podemos usar el primer parámetro Html.TextBox() para especificar el identificador/nombre del elemento y, a continuación, pasar explícitamente en el valor que se usa como segundo parámetro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

A menudo le deseamos realizar un formato personalizado en el valor de salida. El método estático String.Format () integrada en .NET es útil para estos escenarios. Nuestra plantilla de vista Edit.aspx utiliza esto para dar formato al valor EventDate (que es de tipo DateTime) para que no muestra a segundos para el tiempo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un tercer parámetro Html.TextBox() opcionalmente puede usarse para generar los atributos HTML adicionales. El fragmento siguiente muestra cómo representar un tamaño adicional = atributo "30" y una clase = "mycssclass" atributo en el &lt;tipo de entrada = "text" /&gt; elemento. Tenga en cuenta cómo nos estamos secuencias de escape de la clase de atributo utilizando el nombre de un "@" character because "clase" es una palabra reservada en C#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementar el método de acción Edit de HTTP-POST

Ahora tenemos la versión de HTTP-GET de nuestro método de acción Edit implementado. Cuando un usuario solicita la */Dinners/Edit/1* dirección URL que reciben una página HTML similar al siguiente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Al presionar el botón "Guardar" hace que un formulario post a la */Dinners/Edit/1* dirección URL, y envía el HTML &lt;entrada&gt; valores mediante el verbo HTTP POST de formulario. Implementemos ahora el comportamiento de solicitud HTTP POST de nuestro método de acción edit: que se encargará de guardar la cena.

Empezaremos por agregar un método de acción "Editar" sobrecargado a nuestro DinnersController que tiene un atributo de "AcceptVerbs" en lo que indica que controla los escenarios de HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Cuando el atributo [AcceptVerbs] se aplica a los métodos de acción sobrecargado, ASP.NET MVC controla automáticamente las solicitudes de distribución para el método de acción adecuado según el verbo HTTP entrante. Las solicitudes HTTP POST a <em>/dinners/Edit / [id]</em> irán direcciones URL para el método de edición anterior, mientras que todas las demás solicitudes de verbo HTTP para <em>/dinners/Edit / [id]</em>irán direcciones URL para el primer método de edición (lo que hizo implementamos no tiene un atributo [AcceptVerbs]).

| **Tema de lado: ¿Por qué diferencia a través de los verbos HTTP?** |
| --- |
| Puede que se pregunte: ¿por qué estamos usando una sola dirección URL y diferenciar su comportamiento mediante el verbo HTTP? ¿Por qué no solo tiene dos direcciones URL independientes para controlar cargar y guardar los cambios de edición? ¿Por ejemplo: / dinners/Edit / [id] para mostrar el formulario inicial y/dinners/Save / [id] para controlar el envío de formulario para guardarlo? El inconveniente con publicación dos URL independientes es que en los casos donde se publique en /Dinners/Save/2 y, a continuación, deberá volver a mostrar el formulario HTML debido a un error de entrada, el usuario final acabará teniendo la dirección URL de instancias Dinners/Save/2 en la barra de direcciones de su explorador (ya que eso era la dirección URL del formulario enviado al). Si el usuario final coloca una marca en esta página redisplayed a su lista de favoritos del explorador, o copia/pega la dirección URL y envía por correo electrónico a un amigo, terminen guardando una dirección URL que no funcionará en el futuro (ya que esa dirección URL depende de los valores de entrada). Mediante la exposición de una sola dirección URL (como: /Dinners/Edit/[id]) y diferenciar el procesamiento de la misma con el verbo HTTP, es seguro para los usuarios finales a la página de edición de marcador o enviar la dirección URL a otras personas. |

#### <a name="retrieving-form-post-values"></a>Recuperar valores de envío de formulario

Hay varias maneras podemos acceder a registrar los parámetros de formulario dentro de nuestro método HTTP POST de "Editar". Un enfoque sencillo es usar simplemente la propiedad de solicitud en la clase base del controlador para acceder a la colección de formulario y recuperar los valores expuestos directamente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Sin embargo, el enfoque anterior es un poco complicados, especialmente cuando se agregue lógica de control de errores.

Un enfoque más satisfactorio para este escenario es aprovechar la integrada *UpdateModel()* método auxiliar en la clase base del controlador. Admite la actualización de las propiedades de un objeto que se pasa con los parámetros de formulario entrante. Usa la reflexión para determinar los nombres de propiedad en el objeto y, a continuación, automáticamente convierte y asigna valores a ellos según los valores de entrada enviados por el cliente.

Podríamos usar el método UpdateModel() para simplificar la acción de HTTP-POST editar mediante este código:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Ahora podemos visitemos el */Dinners/Edit/1* dirección URL y el título de la cena de cambio:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Cuando hacemos clic en el botón "Guardar", se llevarán a cabo un formulario post a la acción de edición y se conservarán los valores actualizados en la base de datos. Hemos, a continuación, se redirigirán a la dirección URL de detalles para la cena (que se mostrará los valores recién guardados):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Edición de control de errores

Nuestro actual funciona de implementación de HTTP-POST correctamente, excepto cuando hay errores.

Cuando un usuario comete un error editando un formulario, se tiene que asegurarse de que se vuelve a mostrar el formulario con un mensaje de error informativo que les orienta para corregir este problema. Esto incluye los casos donde un usuario final envía una entrada incorrecta (por ejemplo: una cadena de fecha con formato incorrecto), como así como casos donde el formato de entrada es válido pero no hay una infracción de regla de negocio. Cuando se producen errores que del formulario debe conservar los datos de entrada especificado originalmente para que no tienen que rellenar manualmente los cambios realizados por el usuario. Debe repetir este proceso tantas veces como sea necesario hasta que el formulario se complete correctamente.

ASP.NET MVC incluye algunas características útiles integradas que hacen que el control de errores y volver a mostrar de forma fácil. Para ver estas características en acción vamos a actualizar nuestro método de acción Edit con el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

El código anterior es similar a la implementación anterior, salvo que ahora nos estamos ajuste un bloque de control de errores try/catch alrededor de nuestro trabajo. Si se produce una excepción al llamar a UpdateModel() o cuando se pruebe y guarde la instancia DinnerRepository (que se producirá una excepción si el objeto Dinner que estamos intentando guardar no es válido debido a una infracción de regla dentro de nuestro modelo), volverá a nuestro bloque de control de errores de catch ejecutar. Dentro de él se recorrer las infracciones de reglas que existen en el objeto de la cena y agréguelos a un objeto ModelState (que explicaremos en breve). A continuación, se vuelva a mostrar la vista.

Para ver esto trabajar vamos a volver a ejecutar la aplicación, editar una cena y cámbielo para tener un título vacío, un EventDate de "BOGUS" y usar un número de teléfono de Reino Unido con un valor de país de Estados Unidos. Cuando se presiona el botón "Guardar" nuestro método HTTP POST editar no podrá guardar la cena (porque hay errores) y se volverá a mostrar el formulario:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Nuestra aplicación tiene una experiencia de error decente. Los elementos de texto con la entrada no válida se resaltan en rojo y los mensajes de error de validación se muestran al usuario final sobre ellos. El formulario también conserva los datos de entrada que el usuario especificó originalmente – para que no tengan reabastecimiento de nada.

¿Cómo hacerlo, puede que se pregunte, ocurre esto? ¿Cómo los cuadros de texto de título, EventDate y Teléfonodecontacto propios resaltar en rojo y saber para generar los valores de usuario especificado originalmente? ¿Y cómo se muestran los mensajes de error en la lista en la parte superior? La buena noticia es que esto no sucedió por arte de magia, en su lugar fue porque hemos utilizado algunas de las características integradas de ASP.NET MVC que facilitan la validación de entrada y de error escenarios de control.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Understanding ModelState y los métodos de aplicación auxiliar HTML de validación

Las clases de controlador tienen una colección de propiedades "ModelState" que proporciona una manera de indicar que hay errores con un objeto de modelo que se pasan a una vista. Las entradas de error dentro de la colección ModelState identifican el nombre de la propiedad de modelo con el problema (por ejemplo: "Title", "EventDate" o "Teléfonodecontacto") y permitir que un mensaje de error fácil de usar que se especifique (por ejemplo: "Es necesario un título").

El *UpdateModel()* método auxiliar rellena automáticamente la colección ModelState cuando encuentra errores al intentar asignar valores de formulario a propiedades en el objeto de modelo. Por ejemplo, nuestro cena EventDate propiedad de objeto es de tipo DateTime. Cuando el método UpdateModel() no pudo asignarle el valor de cadena "BOGUS" en el escenario anterior, el método UpdateModel() agrega una entrada a la colección ModelState que indica un error de asignación se hubiera producido con esa propiedad.

Los desarrolladores también pueden escribir código para agregar explícitamente las entradas de error en la colección ModelState como hacemos a continuación dentro de nuestro bloque de control de errores de "catch", que rellena la colección ModelState con entradas en función de las infracciones de reglas activas en el Objeto de la cena:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integración de la aplicación auxiliar HTML con ModelState

Métodos auxiliares HTML - como Html.TextBox() - comprueban la colección ModelState al representar la salida. Si hay un error para el elemento, se representan el valor escrito por el usuario y una clase de error CSS.

Por ejemplo, en la vista "Editar" estamos usando el método auxiliar Html.TextBox() para representar el EventDate de nuestro objeto cena:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Cuando se represente la vista en el escenario de error, el método Html.TextBox() comprueba la colección ModelState para ver si hay algún error asociado a la propiedad "EventDate" de nuestro objeto de la cena. Cuando determina que se produjo un error representa la entrada de usuario enviado ("BOGUS") como el valor y agrega una clase de error de css para el &lt;tipo de entrada = "cuadro de texto" /&gt; marcado que ha generado:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Puede personalizar la apariencia de la clase de error de css para buscar el que desee. La clase de error CSS de forma predeterminada: "entrada-error de validación": se define en el *\content\site.css* hoja de estilos y tiene un aspecto similar a la siguiente:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Esta regla CSS es lo que ha provocado nuestros elementos de entrada no válidos se resalten como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Método auxiliar de Html.ValidationMessage()

El método auxiliar Html.ValidationMessage() puede usarse para generar el mensaje de error ModelState asociado con una propiedad de modelo determinado:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Genera el código anterior:  *&lt;abarcan clase = "error de validación de campo"&gt; no es válido el valor 'BOGUS' &lt; /span&gt;*

El método auxiliar Html.ValidationMessage() también admite un segundo parámetro que permite que los desarrolladores reemplacen el mensaje de texto de error que se muestra:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Genera el código anterior:  <em>&lt;abarcan clase = "error de validación de campo"&gt;\*&lt;/span&gt;</em>en lugar del texto de error de forma predeterminada, cuando hay un error para el Propiedad EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Método auxiliar de Html.ValidationSummary()

El método auxiliar Html.ValidationSummary() puede usarse para representar un mensaje de error de resumen, acompañado de un &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; lista de error detallado de todos los mensajes en el Colección ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

El método auxiliar Html.ValidationSummary() toma un parámetro de cadena opcional, que define un mensaje de resumen de error que se muestra encima de la lista de errores detallados:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

También puede usar CSS para invalidar el aspecto de la lista de errores.

#### <a name="using-a-addruleviolations-helper-method"></a>Mediante un método auxiliar AddRuleViolations

Nuestra implementación inicial de editar HTTP-POST usa una instrucción foreach dentro de su bloque catch para recorrer las infracciones de reglas del objeto Dinner y agréguelos a la colección de ModelState del controlador:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Podemos hacer que este código un poco limpiador agregando un "ControllerHelpers" clase al proyecto de NerdDinner e implementar un método de extensión "AddRuleViolations" dentro de él que agrega un método auxiliar a la clase de ASP.NET MVC ModelStateDictionary. Este método de extensión puede encapsular la lógica necesaria para rellenar el ModelStateDictionary con una lista de errores de RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

A continuación, podemos actualizar nuestro método de acción Edit de HTTP-POST para usar este método de extensión para rellenar la colección ModelState con nuestro infracciones de reglas de la cena.

#### <a name="complete-edit-action-method-implementations"></a>Completar las implementaciones de método de acción de edición

El código siguiente implementa toda la lógica de controlador necesaria para nuestro escenario de edición:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Lo bueno de nuestra implementación de la edición es que nuestra clase de controlador ni la plantilla de vista tiene que saber nada acerca de la validación específica o las reglas de negocios que se aplica al nuestro modelo de la cena. Se pueden agregar reglas adicionales a nuestro modelo en el futuro y no es necesario realizar cambios de código a nuestro controlador o vista en orden para que puedan ser compatibles. Esto nos proporciona la flexibilidad necesaria para evolucionar rápidamente nuestros requisitos de aplicaciones en el futuro con un mínimo de cambios de código.

### <a name="create-support"></a>Creación de soporte técnico

Hemos terminado de implementar el comportamiento de nuestra clase DinnersController en "Editar". Ahora pasemos a implementar la compatibilidad de "Crear" en él, lo que permitirá a los usuarios agregar instancias Dinners nuevo.

#### <a name="the-http-get-create-action-method"></a>HTTP-GET crear método de acción

Comenzaremos por implementar el comportamiento HTTP "GET" de nuestro método de acción de crear. Este método se llamará cuando alguien visite la */Dinners/crear* dirección URL. Nuestra implementación tiene el siguiente aspecto:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

El código anterior crea un nuevo objeto Dinner y asigna su propiedad EventDate sea una semana en el futuro. A continuación, representa una vista que se basa en el nuevo objeto de la cena. Dado que no hemos pasado explícitamente un nombre para el *View()* método auxiliar, utilizará la ruta de acceso de forma predeterminada basada en convenciones para resolver la plantilla de vista: /Views/Dinners/Create.aspx.

Vamos a crear ahora esta plantilla de vista. Podemos hacer esto con el botón secundario en el método de acción Create y seleccionando el comando del menú contextual "Agregar vista". En el cuadro de diálogo "Agregar vista" indicaremos que se pasa un objeto de la cena a la plantilla de vista y elegir una plantilla de "Crear" automática scaffolding:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio guardar una nueva vista "Create.aspx" en función de scaffold en el directorio "\Views\Dinners" y abra el IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Vamos a realizar algunos cambios al "crear" scaffold archivo predeterminado que se generó para nosotros y modificar seguridad para el siguiente aspecto:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Y ahora cuando ejecutamos nuestra aplicación y el acceso a la *"/ Dinners/Create"* URL dentro del explorador que representará la interfaz de usuario, como por debajo de nuestra implementación de la acción de creación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementar el HTTP-POST crear método de acción

Tenemos la versión de HTTP-GET de nuestro método de acción Create implementado. Cuando un usuario hace clic en el botón "Guardar" realiza un formulario post a la */Dinners/Create* dirección URL, y envía el HTML &lt;entrada&gt; valores mediante el verbo HTTP POST de formulario.

Implementemos ahora el comportamiento de solicitud HTTP POST de nuestro método de acción de crear. Empezaremos por agregar un método de acción "Crear" sobrecargado a nuestro DinnersController que tiene un atributo de "AcceptVerbs" en lo que indica que controla los escenarios de HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Hay varias maneras podemos acceder a los parámetros de formulario enviados dentro de nuestro método de "Crear" habilitado HTTP-POST.

Un enfoque consiste en crear un nuevo objeto Dinner y, a continuación, use el *UpdateModel()* método de aplicación auxiliar (como se hizo con la acción de edición) para rellenarla con los valores de formulario enviados. Nos podemos agregarlo a nuestra instancia DinnerRepository, almacenar los datos en la base de datos y redirigir al usuario a la acción de detalles para mostrar la cena recién creada con el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Como alternativa, podemos usar un enfoque donde tenemos nuestro método de acción Create() toman un objeto de la cena como un parámetro de método. ASP.NET MVC se, a continuación, automáticamente crear instancias de un nuevo objeto de la cena para nosotros, rellenar sus propiedades en las entradas de formulario y pasarlo a nuestro método de acción:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

El método de acción anterior comprueba que el objeto de la cena se ha rellenado correctamente con los valores del formulario post en el comprobando la propiedad ModelState.IsValid. Esto devolverá false si no hay problemas de conversión de entrada (por ejemplo: una cadena de "BOGUS" para la propiedad EventDate), y si hay algún problema nuestro método de acción vuelve a mostrar el formulario.

Si los valores de entrada son válidos, a continuación, el método de acción intenta agregar y guardar la cena nuevo en la instancia DinnerRepository. Que contiene este trabajo dentro de un bloque try/catch y vuelve a mostrar el formulario si hay cualquier infracción de regla de negocio (lo que provocaría el método dinnerRepository.Save() generar una excepción).

Para ver este comportamiento en acción de control de errores, nos podemos solicitar el */Dinners/crear* dirección URL y rellene los detalles sobre una cena nuevo. Entrada incorrecta o valores hará que el formulario de creación que se vuelve a mostrar con los errores que se resaltan como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Tenga en cuenta cómo nuestro formulario de creación está respetando las reglas de validación y business exactas mismas como nuestro formulario de edición. Esto es porque nuestras reglas de validación y business se han definido en el modelo y no se incrustan dentro de la interfaz de usuario o el controlador de la aplicación. Esto significa que podemos más adelante cambie/evolucionamos nuestro validación o las reglas de negocios en una sola colocan y pídale que se aplican en toda nuestra aplicación. No se tendrá que cambiar ningún código dentro de los nuestro editar o crear métodos de acción para que admita automáticamente las nuevas reglas o modificaciones a las ya existentes.

Cuando se corrección los valores de entrada y haga clic en el botón "Guardar", nuestra adición a la instancia DinnerRepository se realizará correctamente y se agregará una cena nuevo a la base de datos. A continuación, se redirigirá a la */dinners/detalles / [id]* URL-donde se le mostrará los detalles acerca de la cena recién creado:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Eliminar el soporte técnico

Ahora agreguemos soporte técnico de "Eliminar" para nuestro DinnersController.

#### <a name="the-http-get-delete-action-method"></a>El método de acción Delete de HTTP-GET

Comenzaremos por implementar el comportamiento de HTTP GET de nuestro método de acción delete. Este método se llamará cuando alguien visite la */dinners/Delete / [id]* dirección URL. Esta es la implementación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

El método de acción intenta recuperar la cena va a eliminar. Si la cena no existe, presenta una vista se basa en el objeto de la cena. Si el objeto no existe (o ya se ha eliminado) se devuelve una vista que representa el valor de "NotFound" ver plantilla que creó anteriormente para el método de acción "Detalles".

Podemos crear la plantilla de vista "Delete" con el botón secundario en el método de acción Delete y seleccionando el comando del menú contextual "Agregar vista". En el cuadro de diálogo "Agregar vista" indicaremos que se está pasando un objeto de la cena a nuestra plantilla de vista como su modelo y optar por crear una plantilla vacía:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio agregará un nuevo archivo de plantilla de vista de "Delete.aspx" para nosotros en nuestro directorio "\Views\Dinners". Vamos a agregar a la plantilla para implementar una pantalla de confirmación eliminar algún HTML y código como el siguiente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

El código anterior muestra el título de la cena eliminarse y las salidas una &lt;formulario&gt; elemento que realiza una publicación a la URL de/dinners/Delete / [id] si el usuario final hace clic en el botón "Eliminar" dentro de él.

Cuando ejecutamos nuestra aplicación y el acceso a la *"/ Dinners/Delete / [id]"* dirección URL para una cena válida de objeto representa la interfaz de usuario similar a la siguiente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Tema de lado: ¿Por qué hacemos una publicación?** |
| --- |
| Puede que se pregunte: ¿por qué decidimos mediante el esfuerzo de crear un &lt;formulario&gt; dentro de nuestra pantalla de confirmación de eliminación? ¿Por qué no usar simplemente un hipervínculo estándar para vincular a un método de acción que realiza la operación de eliminación real? La razón es que queremos tener cuidado para protegerse frente a los rastreadores web y los motores de detección nuestras direcciones URL y sin darse cuenta que los datos se puede eliminar cuando siguen los vínculos de búsqueda. HTTP-GET según las direcciones URL se consideran "seguras" para que puedan acceso/rastreo y se supone que no siga los HTTP-POST. Una buena norma es para asegurarse de que se ha de colocar siempre destructivo u operaciones de modificación de datos detrás de las solicitudes HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementación del método de acción Delete de HTTP-POST

Ahora tenemos la versión de HTTP-GET de nuestro método de acción Delete implementado que muestra una pantalla de confirmación de eliminación. Cuando un usuario final hace clic en el botón "Eliminar" realizará un formulario post a la */dinners/Dinner / [id]* dirección URL.

Implementemos ahora el comportamiento HTTP "POST" del método de acción delete con el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La versión de HTTP POST de nuestro método de acción de eliminación intenta recuperar el objeto de la cena va a eliminar. Si no se puede encontrar (porque ya se ha eliminado) representa la plantilla de "NotFound". Si no encuentra la cena, lo elimina de la instancia DinnerRepository. A continuación, representa una plantilla de "Eliminado".

Para implementar la plantilla "Eliminado" se haga clic en el método de acción y elija el menú contextual "Agregar vista". Se le asigne un nombre nuestra vista "Eliminado" y tiene ser una plantilla vacía (y no aceptan un objeto de modelo fuertemente tipado). A continuación, vamos a agregar algún contenido HTML a él:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Y ahora cuando ejecutamos nuestra aplicación y el acceso a la *"/ Dinners/Delete / [id]"* dirección URL para una cena válida confirmación de eliminación de objeto que representará la cena de pantalla como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Cuando hacemos clic en el botón "Eliminar" realizará una solicitud HTTP POST a la */dinners/Delete / [id]* dirección URL, que se elimine la cena de nuestra base de datos y mostrará la plantilla de vista "Eliminado":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Seguridad del modelo de enlace

Hemos hablado de dos maneras diferentes de utilizar las características integradas de enlace de modelos de ASP.NET MVC. El primero mediante el método UpdateModel() para actualizar las propiedades en un objeto de modelo existente y el segundo mediante compatibilidad de ASP.NET MVC para pasar objetos de modelo de como parámetros de método de acción. Ambas técnicas son muy eficaces y muy útil.

Esta capacidad también conlleva responsabilidad. Es importante que siempre sea paranoicos acerca de la seguridad al aceptar la intervención del usuario, y lo mismo sucede al enlazar objetos a la entrada del formulario. Deben extremarse las precauciones al HTML codificar siempre los valores introducidos por el usuario para evitar ataques por inyección de código HTML y JavaScript y tenga cuidado de ataques de inyección SQL (Nota: estamos usando LINQ to SQL para nuestra aplicación, que codifica automáticamente parámetros para evitar estos tipos de ataques). Nunca debe confiar en la validación del lado cliente sólo y siempre se emplee validación del lado servidor para protegerse contra piratas informáticos que intentan enviar valores falsos.

Un elemento de seguridad adicional para asegurarse de que pensar al usar las características de enlace de ASP.NET MVC es el ámbito de los objetos que se va a enlazar. En concreto, desea asegurarse de que comprende las implicaciones de seguridad de las propiedades que se va a permitir enlazarse y asegúrese de que solo permite que las propiedades que realmente deben ser actualizables por un usuario final para actualizarse.

De forma predeterminada, el método UpdateModel() intentará actualizar todas las propiedades en el objeto de modelo que coinciden con los valores de parámetro de formulario entrante. Del mismo modo, pueden tener objetos pasados como parámetros de método de acción también de forma predeterminada todas las propiedades establecidas a través de los parámetros de formulario.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Bloqueo de enlace en una base por uso

Puede bloquear la directiva de enlace en una base por uso por lo que proporciona un explícita "incluir lista" de propiedades que se pueden actualizar. Esto puede realizarse pasando un parámetro de matriz de cadena adicional al método UpdateModel() como a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objetos que se pasan como parámetros de método de acción también admiten un atributo [Bind] que permite un "incluir lista" de permiten a las propiedades que se especifique como a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bloqueo de enlace según el tipo

También puede bloquear las reglas de enlace por tipo. Esto permite especificar las reglas de enlace una vez y, a continuación, haga que se aplican en todos los escenarios (incluidos los escenarios de parámetro de método UpdateModel y acción) en todos los controladores y métodos de acción.

Puede personalizar las reglas de enlace por tipo mediante la adición de un atributo [Bind] en un tipo o registrarlo en el archivo Global.asax de la aplicación (útil para escenarios donde no tiene el tipo). A continuación, puede usar Include el enlace del atributo y las propiedades de exclusión para controlar las propiedades que son enlazables para la interfaz o clase en particular.

Se deberá usar esta técnica para la clase Dinner en nuestra aplicación NerdDinner y agregue un atributo [Bind] que restringe la lista de propiedades enlazables a la siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Tenga en cuenta no permitimos la colección de confirmaciones de asistencia para su manipulación a través del enlace, ni tampoco se permite que las propiedades DinnerID o HostedBy debe establecerse a través del enlace. Por motivos de seguridad en su lugar, se podrán manipular solo estas propiedades determinadas mediante código explícito en nuestros métodos de acción.

### <a name="crud-wrap-up"></a>Resumen CRUD

ASP.NET MVC incluye una serie de características integradas que ayudan con la implementación de escenarios de registro del formulario. Se usa una variedad de estas características para proporcionar compatibilidad de UI de CRUD sobre nuestra instancia DinnerRepository.

Estamos usando un enfoque centrado en el modelo para implementar nuestra aplicación. Esto significa que toda la validación y lógica se define dentro de nuestro nivel de modelo y no dentro de nuestros controladores o vistas de regla de negocios. Nuestra clase de controlador ni nuestras plantillas de vista sepa nada acerca de las reglas de negocios específicas que se aplica al nuestra clase de modelo de la cena.

Esto mantendrá limpia nuestra arquitectura de aplicación y que sea más fácil probar. Podemos agregar reglas de negocio adicionales a la capa del modelo en el futuro y *no tiene que realizar cambios de código* a nuestro controlador o vista en orden para ellos que deben admitir. Esto va a proporcionar una gran cantidad de agilidad a evolucionan y cambian de nuestra aplicación en el futuro.

Nuestro DinnersController ahora permite cena anuncios y detalles, así como crear, editar y eliminar soporte técnico. El código completo de la clase encontrará a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Paso siguiente

Ahora tenemos compatibilidad básica de CRUD (creación, lectura, actualización y eliminación) implementar dentro de nuestra clase DinnersController.

Ahora veamos cómo podemos usar clases ViewData y ViewModel para habilitar la interfaz de usuario incluso más completa en los formularios.

> [!div class="step-by-step"]
> [Anterior](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Siguiente](use-viewdata-and-implement-viewmodel-classes.md)
