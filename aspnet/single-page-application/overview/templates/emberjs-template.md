---
uid: single-page-application/overview/templates/emberjs-template
title: Plantilla de EmberJS | Documentos de Microsoft
author: xqiu
description: Plantilla de EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506804"
---
<a name="emberjs-template"></a>Plantilla de EmberJS
====================
por [Xinyang Qiu](https://github.com/xqiu)

> La plantilla de MVC EmberJS escribe Nathan Totten, Thiago Santos y Xinyang Qiu.
> 
> [Descargue la plantilla de MVC EmberJS](https://go.microsoft.com/fwlink/?LinkId=282647)


La plantilla de SPA EmberJS está diseñada para ayudarle a comenzar a crear rápidamente aplicaciones web de cliente interactivas mediante EmberJS.

"Aplicación de página" (contraseña segura SPA) es el término general para una aplicación web que se carga una página HTML única y, a continuación, actualiza la página de forma dinámica, en lugar de cargar páginas nuevas. Después de la carga de la página inicial, la aplicación SPA se comunica con el servidor a través de solicitudes de AJAX.

![](emberjs-template/_static/image1.png)

AJAX no es nada nuevo, pero en la actualidad existen marcos de JavaScript que resulten más fácil generar y mantener una gran aplicación SPA sofisticada. Además, HTML5 y CSS3 se conseguir que sea más fácil de crear interfaces de usuario enriquecidas.

La plantilla de SPA EmberJS utiliza el [Ember](http://emberjs.com/) biblioteca de JavaScript para controlar las actualizaciones de la página de solicitudes de AJAX. Ember.js usa el enlace de datos para sincronizar la página con los datos más recientes. De este modo, no tienes que escribir el código que le guía a través de los datos JSON y actualiza el DOM. En su lugar, coloque atributos declarativos en el código HTML que indican Ember.js cómo presentar los datos.

En el servidor, es casi idéntica a la plantilla de EmberJS la [plantilla KnockoutJS SPA](../introduction/knockoutjs-template.md). Usa ASP.NET MVC para servir documentos HTML y ASP.NET Web API para controlar las solicitudes de AJAX del cliente. Para obtener más información sobre los aspectos de la plantilla, consulte el [KnockoutJS plantilla](../introduction/knockoutjs-template.md) documentación. En este tema se centra en las diferencias entre la plantilla de Knockout y la plantilla EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Crear un proyecto de plantilla SPA EmberJS

Descargue e instale la plantilla, haga clic en el botón de descarga más arriba. Debe reiniciar Visual Studio.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto y haga clic en **Aceptar**.

![](emberjs-template/_static/image2.png)

En el **nuevo proyecto** asistente, seleccione **Ember.js SPA proyecto**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Introducción a la plantilla EmberJS SPA

La plantilla de EmberJS utiliza una combinación de jQuery, Ember.js, Handlebars.js para crear una interfaz de usuario interactivo, suave.

Ember.js es una biblioteca de JavaScript que use un patrón MVC del lado cliente.

- A *plantilla*escrito en el lenguaje de definición de plantillas manillares, describe la interfaz de usuario de la aplicación. En modo de lanzamiento, el [compilador manillares](https://github.com/Myslik/csharp-ember-handlebars) se utiliza para agrupar y compilar la plantilla manillares.
- A *modelo* almacena los datos de aplicación que obtiene del servidor (listas de tareas y elementos de lista de tareas).
- A *controlador* almacena el estado de la aplicación. A menudo, controladores de presentan datos de modelo a las plantillas correspondientes.
- A *vista* traduce primitivos eventos de la aplicación y los pasa al controlador.
- A *enrutador* administra el estado de aplicación, manteniendo las direcciones URL y las plantillas sincronizadas.

Además, la biblioteca de datos de Ember puede utilizarse para sincronizar objetos JSON (obtenidos del servidor a través de la API de REST) y los modelos de cliente.

La plantilla EmberJS SPA organiza las secuencias de comandos en ocho capas:

- webapi\_adapter.js, webapi\_serializer.js: extiende la biblioteca de datos de Ember para que funcione con ASP.NET Web API.
- Scripts/helpers.js: Define las aplicaciones auxiliares Ember manillares nueva.
- Scripts/App.js: La aplicación se crea y configura el adaptador y el serializador.
- Las secuencias decomandos/aplicación/modelos/\*.js: define los modelos.
- Las secuencias decomandos/aplicación/vistas/\*.js: define las vistas.
- Las secuencias decomandos/aplicación/controladores/\*.js: define los controladores.
- Las secuencias de comandos/aplicación/rutas, Scripts/app/router.js: Define las rutas.
- Plantillas /\*.hbs: define las plantillas manillares.

Echemos un vistazo a algunas de estas secuencias de comandos con más detalle.

## <a name="models"></a>Modelos

Los modelos se definen en la carpeta Scripts/aplicación/models. Hay dos archivos de modelo: todoItem.js y todoList.js.

**todo.Model.js** define los modelos de cliente (explorador) para las listas de tareas pendientes. Hay dos clases de modelo: todoItem y todoList. En Ember, los modelos son subclases de DS. Modelo. Un modelo puede tener propiedades con atributos:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Los modelos pueden definir relaciones con otros modelos:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Los modelos pueden calcular propiedades que se enlazan a otras propiedades:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Los modelos pueden tener funciones de observador, que se invocan cuando cambia una propiedad observada:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Vistas

Las vistas se definen en la carpeta Scripts/aplicación/vistas. Una vista convierte los eventos del interfaz de usuario de la aplicación. Un controlador de eventos puede devolver la llamada a funciones de controlador o simplemente llamar directamente al contexto de datos.

Por ejemplo, el código siguiente es de views/TodoItemEditView.js. Define el control de eventos para un campo de texto de entrada.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controlador

Los controladores se definen en la carpeta de secuencias de comandos/aplicación/controladores. Para representar un único modelo, extender `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Un controlador también puede representar una colección de modelos mediante la extensión `Ember.ArrayController`. Por ejemplo, el TodoListController representa una matriz de `todoList` objetos. El controlador se ordena por identificador de todoList, en orden descendente:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

El controlador define una función denominada `addTodoList`, que crea un nuevo todoList y lo agrega a la matriz. Para ver cómo llama a esta función, abra el archivo de plantilla denominado todoListTemplate.html, en la carpeta de plantillas. El siguiente código de plantilla enlaza un botón a la `addTodoList` función:

[!code-html[Main](emberjs-template/samples/sample8.html)]

El controlador también contiene un `error` propiedad, que contiene un mensaje de error. Este es el código de plantilla para mostrar el mensaje de error (también en todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Rutas

Router.js define las rutas y la plantilla predeterminada para mostrar, Establece el estado de aplicación y coincide con las direcciones URL a las rutas:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js carga datos para el TodoListRoute reemplazando la función setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember usa las convenciones de nomenclatura para que coincida con las direcciones URL, los nombres de ruta, controladores y plantillas. Para obtener más información, consulte [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) en la documentación de EmberJS.

## <a name="templates"></a>Plantillas

La carpeta de plantillas contiene cuatro plantillas:

- Application.HBS: la plantilla predeterminada que se representa cuando se inicia la aplicación.
- About.HBS: la plantilla de la ruta "/ punto".
- index.HBS: la plantilla para la raíz de la ruta "/".
- todoList.hbs: la plantilla para la "/ todo" ruta.
- \_NavBar.HBS: la plantilla define el menú de navegación.

La plantilla de aplicación actúa como una página maestra. Contiene un encabezado y un pie de página, una "{{toma}}" para insertar otras plantillas de función de la ruta. Para obtener más información acerca de las plantillas de aplicación en Ember, consulte [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

El "/ todoList" plantilla contiene dos expresiones de bucle. El bucle exterior es `{{#each controller}}`y el interior bucle es `{{#each todos}}`. El código siguiente muestra un integrada `Ember.Checkbox` ver una personalizada `App.TodoItemEditView`y un vínculo con un `deleteTodo` acción.

[!code-html[Main](emberjs-template/samples/sample12.html)]

El `HtmlHelperExtensions` (clase), definida en Controllers/HtmlHelperExensions.cs, define una aplicación auxiliar de función para almacenar en memoria caché e Insertar plantilla archivos al **depurar** está establecido en **true** en el archivo Web.config. Esta función se llama desde el archivo de vista de MVC de ASP.NET definido en Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Se llama sin argumentos, la función representa todos los archivos de plantilla en la carpeta de plantillas. También puede especificar una subcarpeta o un archivo de plantilla específica.

Cuando **depurar** es **false** en el archivo Web.config, la aplicación incluye el elemento de agrupación "~/bundles/templates". Este elemento de agrupación se agrega en BundleConfig.cs, mediante la biblioteca de compilador manillares:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
