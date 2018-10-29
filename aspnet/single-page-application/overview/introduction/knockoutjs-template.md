---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Aplicación de página única: Plantilla KnockoutJS | Microsoft Docs'
author: MikeWasson
description: Plantilla de knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 60bc8bf95cace722244ffc87ff4c00126a0ed2a0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207451"
---
<a name="single-page-application-knockoutjs-template"></a>Aplicación de página única: Plantilla KnockoutJS
====================
por [Mike Wasson](https://github.com/MikeWasson)

> La plantilla de Knockout MVC forma parte de ASP.NET y Web Tools 2012.2
> 
> [Descargar ASP.NET y Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


La actualización ASP.NET y Web Tools 2012.2 incluye una plantilla de aplicación de página única (SPA) para ASP.NET MVC 4. Esta plantilla está diseñada para ayudarle a comenzar a crear rápidamente aplicaciones web interactivas del lado cliente.

"Aplicación de página única" (SPA) es el término general para una aplicación web que se carga una página HTML única y, a continuación, actualiza la página de forma dinámica, en lugar de cargar páginas nuevas. Después de la carga de página inicial, la SPA se comunica con el servidor a través de las solicitudes AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX no es nada nuevo, pero hoy en día existen marcos de JavaScript que resulte más fácil crear y mantener una gran aplicación SPA sofisticada. Además, HTML5 y CSS3 son más fácil crear interfaces de usuario enriquecidas.

Para ayudarle a comenzar, la plantilla SPA crea una aplicación de ejemplo "Lista de tareas pendientes". En este tutorial, echaremos una visita guiada de la plantilla. Primero se deberá mirar la propia aplicación de lista de tareas pendientes y después examine las partes de la tecnología que hacen que funcione.

## <a name="create-a-new-spa-template-project"></a>Cree un nuevo proyecto de plantilla SPA

Requisitos:

- Visual Studio 2012 o Visual Studio Express 2012 para Web
- Herramientas Web de ASP.NET 2012.2 actualizan. Puede instalar la actualización [aquí](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Inicie Visual Studio y seleccione **nuevo proyecto** desde la página de inicio. O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto y haga clic en **Aceptar**.

![](knockoutjs-template/_static/image2.png)

En el **nuevo proyecto** asistente, seleccione **aplicación de página única**.

![](knockoutjs-template/_static/image3.png)

Presione F5 para compilar y ejecutar la aplicación. Cuando la aplicación se ejecuta en primer lugar, muestra una pantalla de inicio de sesión.

![](knockoutjs-template/_static/image4.png)

Haga clic en el &quot;registrarse&quot; vincular y crear un nuevo usuario.

![](knockoutjs-template/_static/image5.png)

Después de iniciar sesión, la aplicación crea una lista de tareas predeterminada con dos elementos. Haga clic en "Agregar la lista de tareas pendientes" para agregar una nueva lista.

![](knockoutjs-template/_static/image6.png)

Cambiar el nombre de la lista, agregar elementos a la lista y desactivarlas. También puede eliminar elementos o eliminar una lista completa. Los cambios se guardan automáticamente en una base de datos en el servidor (realmente LocalDB en este momento, ya que la aplicación se ejecuta localmente).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Arquitectura de la plantilla SPA

Este diagrama muestra los principales bloques de creación de la aplicación.

![](knockoutjs-template/_static/image8.png)

En el servidor, ASP.NET MVC sirve el código HTML y también controla la autenticación basada en formularios.

ASP.NET Web API controla todas las solicitudes relacionadas con el ToDoLists y ToDoItems, incluida la introducción, crear, actualizar y eliminar. El cliente intercambia datos con Web API en formato JSON.

Entity Framework (EF) es la capa O/RM. Actúa como mediador entre el mundo orientada a objetos de ASP.NET y la base de datos subyacente. La base de datos utiliza LocalDB, pero puede cambiarlo en el archivo Web.config. Normalmente usaría LocalDB para el desarrollo local y, a continuación, implementar en una base de datos SQL en el servidor, mediante la migración de EF code first.

En el lado cliente, la biblioteca Knockout.js controla las actualizaciones de la página de las solicitudes AJAX. Knockout usa el enlace de datos para sincronizar la página con los datos más recientes. De este modo, no tendrá que escribir ningún código que le guía a través de los datos JSON y actualiza el DOM. En su lugar, coloque atributos declarativos en el código HTML que indican cómo presentar los datos de Knockout.

Una gran ventaja de esta arquitectura es que separa la capa de presentación de la lógica de aplicación. Puede crear la parte de la API Web sin necesidad de saber nada acerca de cómo se verá la página web. En el lado cliente, crear un modelo de"vista" para representar tales datos y el modelo de vista utiliza Knockout para enlazar con el código HTML. Que le permite cambiar fácilmente el código HTML sin cambiar el modelo de vista. (Analizaremos Knockout un poco más adelante.)

## <a name="models"></a>Modelos

En el proyecto de Visual Studio, la carpeta Models contiene los modelos que se usan en el servidor. (También hay modelos en el lado cliente; nos ocuparemos de estas).

![](knockoutjs-template/_static/image9.png)

**TodoItem, TodoList**

Estos son los modelos de base de datos de Entity Framework Code First. Tenga en cuenta que estos modelos tienen propiedades que señalan entre sí. `ToDoList` contiene una colección de tareas pendientes y cada `ToDoItem` tiene una referencia a su elemento primario ToDoList. Estas propiedades se denominan propiedades de navegación, y que representan a la relación de uno a varios, una lista de tareas pendientes y sus elementos pendientes.

El `ToDoItem` clase también utiliza el **[ForeignKey]** atributo para especificar que `ToDoListId` es una clave externa en el `ToDoList` tabla. Esto indica a EF para agregar una restricción foreign key a la base de datos.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Estas clases definen los datos que se enviará al cliente. "DTO" es el acrónimo "objeto de transferencia de datos". El DTO define cómo las entidades se serializarán en JSON. En general, hay varias razones para usar dto:

- Para controlar qué propiedades se serializan. El DTO puede contener un subconjunto de las propiedades del modelo de dominio. Puede hacerlo por razones de seguridad (ocultar información confidencial) o simplemente para reducir la cantidad de datos que envía.
- Para cambiar la forma de los datos, por ejemplo, para acoplar una estructura de datos más compleja.
- Para mantener cualquier lógica de negocios fuera el DTO (separación de intereses).
- Si no se puede serializar los modelos de dominio por algún motivo. Por ejemplo, las referencias circulares pueden causar problemas cuando se serializa un objeto hay maneras de tratar este problema en la API Web (consulte [referencias a objetos de control Circular](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); pero simplemente usando un DTO evita el problema por completo.

En la plantilla SPA, el dto contiene los mismos datos que los modelos de dominio. Sin embargo, siguen siendo útiles porque evitar referencias circulares desde las propiedades de navegación y demuestran el patrón general de DTO.

**AccountModels.cs**

Este archivo contiene los modelos para la pertenencia al sitio. La `UserProfile` clase define el esquema de perfiles de usuario en la base de datos de pertenencia. (En este caso, la única información que es el identificador de usuario y el nombre de usuario.) Las demás clases de modelo de este archivo se usan para crear el usuario en formularios de inicio de sesión y registro.

## <a name="entity-framework"></a>Entity Framework

La plantilla SPA usa EF Code First. En el desarrollo Code First, definir los modelos en primer lugar en el código y, a continuación, EF usa el modelo para crear la base de datos. También puede usar EF con una base de datos existente ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

El `TodoItemContext` deriva de la clase en la carpeta Models **DbContext**. Esta clase proporciona el "pegamento" entre los modelos y EF. El `TodoItemContext` contiene un `ToDoItem` colección y una `TodoList` colección. Para consultar la base de datos, simplemente hay que escribir una consulta LINQ en estas colecciones. Por ejemplo, mostramos cómo puede seleccionar todas las listas de tareas pendientes para el usuario "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

También puede agregar nuevos elementos a la colección, actualizar elementos, o eliminar elementos de la colección y conservar los cambios en la base de datos.

## <a name="aspnet-web-api-controllers"></a>Controladores de ASP.NET Web API

En ASP.NET Web API, los controladores son objetos que controlan las solicitudes HTTP. Como se mencionó, la plantilla SPA usa la API Web para habilitar las operaciones CRUD en `ToDoList` y `ToDoItem` instancias. Los controladores se encuentran en la carpeta de controladores de la solución.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Controla las solicitudes HTTP de tareas pendientes
- `TodoListController`: Controla las solicitudes HTTP de listas de tareas pendientes.

Estos nombres son considerables, porque la ruta de acceso URI para el nombre del controlador coincide con la API Web. (Para obtener información sobre cómo API Web enruta las solicitudes HTTP a los controladores, consulte [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Echemos un vistazo a la `ToDoListController` clase. Contiene a un miembro de datos única:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

El `TodoItemContext` se utiliza para comunicarse con EF, como se describió anteriormente. Los métodos en el controlador implementan las operaciones CRUD. API Web asigna las solicitudes HTTP desde el cliente a los métodos de controlador, como sigue:

| Solicitud HTTP | Método de controlador | Descripción |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Obtiene una colección de listas de tareas pendientes. |
| GET/API/todo/*Id.* | `GetTodoList` | Obtiene una lista de tareas por Id. |
| PUT/API/todo/*Id.* | `PutTodoList` | Actualiza una lista de tareas pendientes. |
| POST /api/todo | `PostTodoList` | Crea una nueva lista de tareas pendientes. |
| DELETE/API/todo/*Id.* | `DeleteTodoList` | Elimina una lista de tareas. |

Tenga en cuenta que los URI para algunas operaciones contienen marcadores de posición para el valor de identificador. Por ejemplo, para eliminar una lista a con un identificador de 42, el URI es `/api/todo/42`.

Para más información sobre el uso de API Web para las operaciones CRUD, consulte [crear una API Web que admite operaciones de CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). El código de este controlador es bastante sencillo. Estos son algunos puntos interesantes:

- El `GetTodoLists` método usa una consulta LINQ para filtrar los resultados por el identificador del usuario que ha iniciado sesión. De este modo, un usuario solo ve los datos que pertenecen a él o ella. Además, tenga en cuenta que una instrucción Select se usa para convertir el `ToDoList` instancias en `TodoListDto` instancias.
- Los métodos PUT y POST comprueban el estado del modelo antes de modificar la base de datos. Si **ModelState.IsValid** es false, estos métodos devuelven HTTP 400 Solicitud incorrecta. Obtenga más información sobre validación de modelos en Web API en [validación del modelo](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- La clase de controlador también está decorada con el **[Authorize]** atributo. Este atributo se comprueba si se ha autenticado la solicitud HTTP. Si la solicitud no está autenticada, el cliente recibe HTTP 401, no autorizado. Obtenga más información sobre la autenticación en [autenticación y autorización en ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

El `TodoController` es muy similar a la clase `TodoListController`. La principal diferencia es que no define ningún método GET, porque el cliente obtendrá las tareas pendientes, junto con cada lista de tareas pendientes.

## <a name="mvc-controllers-and-views"></a>Vistas y controladores MVC

Los controladores MVC también se encuentran en la carpeta de controladores de la solución. `HomeController` representa el código HTML principal para la aplicación. Se define la vista para el controlador Home en Views/Home/Index.cshtml. La vista principal representa contenido diferente según si el usuario ha iniciado sesión:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Cuando los usuarios iniciar sesión, verán la interfaz de usuario principal. En caso contrario, verán el panel de inicio de sesión. Tenga en cuenta que esta representación condicional se produce en el lado del servidor. Nunca intentan ocultar contenido confidencial en el lado cliente&#8212;todo lo que se envía en una respuesta HTTP es visible para alguien que está inspeccionando los mensajes HTTP sin formato.

## <a name="client-side-javascript-and-knockoutjs"></a>Knockout.js y JavaScript del lado cliente

Ahora dirijamos desde el servidor de la aplicación al cliente. La plantilla SPA usa una combinación de jQuery y Knockout.js para crear una interfaz de usuario interactiva y sin problemas. Knockout.js es una biblioteca de JavaScript que facilita el HTML de enlazar a datos. Knockout.js utiliza un patrón denominado "Model-View-ViewModel".

- El modelo es que los datos del dominio (listas de tareas pendientes y elementos de lista de tareas).
- La vista es el documento HTML.
- El modelo de vista es un objeto de JavaScript que contiene los datos del modelo. El modelo de vista es una abstracción de código de la interfaz de usuario. No tiene ningún conocimiento de la representación de HTML. En su lugar, representa las características abstractas de la vista, como "una lista de tareas pendientes".

La vista está enlazada a datos al modelo de vista. Actualizaciones para el modelo de vista se reflejan automáticamente en la vista. Los enlaces funcionan de la otra dirección. Los eventos de DOM (por ejemplo, cuando hace clic en) están enlazados a datos a las funciones en el modelo de vista que desencadenan las llamadas AJAX.

La plantilla SPA organiza el código de JavaScript del lado cliente en tres capas:

- todo.DataContext.js: envía las solicitudes AJAX.
- todo.Model.js: define los modelos.
- todo.ViewModel.js: define el modelo de vista.

![](knockoutjs-template/_static/image11.png)

Estos archivos de script se encuentran en la carpeta de Scripts o aplicaciones de la solución.

![](knockoutjs-template/_static/image12.png)

**todo.DataContext** controla todas las llamadas de AJAX a los controladores de API Web. (Las llamadas de AJAX para el registro se definen en otra parte, en ajaxlogin.js).

**todo.Model.js** define los modelos del lado cliente (explorador) para las listas de tareas pendientes. Hay dos clases de modelo: todoItem y todoList.

Muchas de las propiedades en las clases del modelo son del tipo "clase observable ko.observable". Los objetos observables son cómo Knockout hace su magia. Desde el [documentación de Knockout](http://knockoutjs.com/documentation/introduction.html): un objeto observable es un "objeto de JavaScript que puede notificar a los suscriptores sobre los cambios." Cuando se cambia el valor de un objeto observable, Knockout actualiza los elementos HTML enlazados a esos objetos observables. Por ejemplo, todoItem tiene objetos observables para las propiedades title y hace:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

También puede suscribirse a un objeto observable en código. Por ejemplo, la clase todoItem se suscribe a los cambios en las propiedades "hace" y "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Modelo de vista**

El modelo de vista se define en todo.viewmodel.js. El modelo de vista es el punto central donde la aplicación enlaza los elementos de la página HTML a los datos de dominio. En la plantilla SPA, el modelo de vista contiene una matriz observable de todoLists. El siguiente código en el modelo de vista le indica a Knockout para aplicar los enlaces:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML y enlace de datos

El HTML de la página principal se define en Views/Home/Index.cshtml. Dado que estamos usando el enlace de datos, el código HTML es solo una plantilla para lo que realmente se representa. Utiliza knockout *declarativa* enlaces. Puede enlazar elementos de la página con datos mediante la adición de un atributo de "data-bind" al elemento. Este es un ejemplo muy sencillo, que se toman de la documentación de Knockout:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

En este ejemplo, Knockout actualiza el contenido de la **&lt;abarcan&gt;** elemento con el valor de `myItems.count()`. Cuando se cambia este valor, Knockout actualiza el documento.

Knockout proporciona una serie de distintos tipos de enlaces. Estos son algunos de los enlaces utilizados en la plantilla SPA:

- **foreach**: le permite recorrer en iteración un bucle y aplicar el mismo marcado para cada elemento de la lista. Esto se utiliza para representar las listas de tareas pendientes y elementos de tareas pendientes. Dentro de la **foreach**, los enlaces se aplican a los elementos de la lista.
- **visible**: usa para alternar la visibilidad. Ocultar marcado cuando una colección está vacía, o hacer visible el mensaje de error.
- **valor**: usado para rellenar los valores del formulario.
- **Haga clic en**: un evento de clic se enlaza a una función en el modelo de vista.

## <a name="anti-csrf-protection"></a>Protección de Anti-CSRF

Falsificación de solicitud entre sitios (CSRF) es un ataque en un sitio malintencionado envía una solicitud a un sitio vulnerable donde el usuario ha iniciado en. Para ayudar a evitar ataques CSRF, ASP.NET MVC usa *tokens antifalsificación*, también se denomina solicitud en tokens de comprobación. La idea es que el servidor asigna un token generado aleatoriamente en una página web. Cuando el cliente envía datos al servidor, debe incluir este valor en el mensaje de solicitud.

Tokens antifalsificación funcionan porque la página malintencionada no puede leer los tokens de usuario, debido a las directivas del mismo origen. (Las directivas del mismo origen evitan documentos hospedados en dos sitios diferentes de obtener acceso al contenido de los demás).

ASP.NET MVC proporciona compatibilidad integrada para tokens antifalsificación, a través de la [antifalsificación](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) clase y el [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) atributo. Actualmente, esta funcionalidad no está integrada en API Web. Sin embargo, la plantilla SPA incluye una implementación personalizada para API Web. Este código se define en el `ValidateHttpAntiForgeryTokenAttribute` (clase), que se encuentra en la carpeta filtros de la solución. Para obtener más información acerca de anti-CSRF en Web API, consulte [ataques de falsificación de solicitud entre sitios impide (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusión

La plantilla SPA está diseñada para ayudarle a comenzar a escribir rápidamente aplicaciones web modernos e interactivos. Usa la biblioteca Knockout.js para separar la presentación (marcado HTML) de la lógica de aplicación y datos. Pero Knockout no es la única biblioteca de JavaScript que puede usar para crear una SPA. Si desea explorar otras opciones, eche un vistazo a la [creados por la Comunidad de plantillas SPA](../templates/index.md).
