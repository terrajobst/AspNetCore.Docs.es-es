---
uid: single-page-application/overview/templates/emberjs-template
title: Plantilla de EmberJS | Microsoft Docs
author: xqiu
description: Plantilla de EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1f5b005180fed15c51b36417cdcd6acc98123c9a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374171"
---
<a name="emberjs-template"></a>Plantilla de EmberJS
====================
por [Xinyang Qiu](https://github.com/xqiu)

> La plantilla de EmberJS MVC está escrita por Nathan Totten, Thiago Santos y Xinyang Qiu.
> 
> [Descargue la plantilla de EmberJS MVC](https://go.microsoft.com/fwlink/?LinkId=282647)


La plantilla de EmberJS SPA está diseñada para ayudarle a comenzar a crear rápidamente aplicaciones web interactivas del lado cliente mediante EmberJS.

"Aplicación de página única" (SPA) es el término general para una aplicación web que se carga una página HTML única y, a continuación, actualiza la página de forma dinámica, en lugar de cargar páginas nuevas. Después de la carga de página inicial, la SPA se comunica con el servidor a través de las solicitudes AJAX.

![](emberjs-template/_static/image1.png)

AJAX no es nada nuevo, pero hoy en día existen marcos de JavaScript que resulte más fácil crear y mantener una gran aplicación SPA sofisticada. Además, HTML5 y CSS3 son más fácil crear interfaces de usuario enriquecidas.

La plantilla de EmberJS SPA usa el [Ember](http://emberjs.com/) biblioteca de JavaScript para controlar las actualizaciones de la página de las solicitudes AJAX. Ember.js usa el enlace de datos para sincronizar la página con los datos más recientes. De este modo, no tendrá que escribir ningún código que le guía a través de los datos JSON y actualiza el DOM. En su lugar, coloque atributos declarativos en el código HTML que indican Ember.js cómo presentar los datos.

En el servidor, es casi idéntica a la plantilla de EmberJS la [plantilla KnockoutJS SPA](../introduction/knockoutjs-template.md). ASP.NET MVC usa para atender a documentos HTML y ASP.NET Web API para controlar las solicitudes AJAX desde el cliente. Para obtener más información sobre los aspectos de la plantilla, consulte el [plantilla KnockoutJS](../introduction/knockoutjs-template.md) documentación. En este tema se centra en las diferencias entre la plantilla de Knockout y la plantilla de EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Crear un proyecto de plantilla de EmberJS SPA

Descargue e instale la plantilla, haga clic en el botón de descarga anterior. Es posible que deba reiniciar Visual Studio.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto y haga clic en **Aceptar**.

![](emberjs-template/_static/image2.png)

En el **nuevo proyecto** asistente, seleccione **Ember.js SPA Project**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Introducción a la plantilla de EmberJS SPA

La plantilla de EmberJS usa una combinación de jQuery, Ember.js, Handlebars.js para crear una interfaz de usuario interactiva y sin problemas.

Ember.js es una biblioteca de JavaScript que usa un patrón MVC del lado cliente.

- Un *plantilla*, escrito en el lenguaje de plantillas Handlebars, se describe la interfaz de usuario de la aplicación. En modo de lanzamiento, el [compilador Handlebars](https://github.com/Myslik/csharp-ember-handlebars) se usa para agrupar y compilar la plantilla de handlebars.
- Un *modelo* almacena los datos de aplicación que obtiene del servidor (listas de tareas pendientes y elementos de lista de tareas).
- Un *controlador* almacena el estado de la aplicación. Los controladores suelen presentan los datos de modelo a las plantillas correspondientes.
- Un *vista* traduce primitivos eventos de la aplicación y los pasa al controlador.
- Un *enrutador* administra el estado de la aplicación, lo que las direcciones URL y las plantillas mantiene sincronizados.

Además, la biblioteca de Ember datos puede utilizarse para sincronizar objetos JSON (obtenidos desde el servidor a través de una API de REST) y los modelos de cliente.

La plantilla de EmberJS SPA organiza las secuencias de comandos en ocho capas:

- webapi\_adapter.js, webapi\_serializer.js: amplía la biblioteca de Ember datos para que funcione con ASP.NET Web API.
- Scripts/helpers.js: Define las nuevas aplicaciones auxiliares de Ember Handlebars.
- Scripts/App.js: Se crea la aplicación y se configura el adaptador y el serializador.
- Las secuencias decomandos/aplicación/modelos/\*.js: define los modelos.
- Vistas, secuencias decomandos/app/\*.js: define las vistas.
- Las secuencias decomandos/aplicación/controladores/\*.js: define los controladores.
- Las secuencias de comandos/aplicación/routes, Scripts/app/router.js: Define las rutas.
- Plantillas /\*.hbs: define las plantillas handlebars.

Echemos un vistazo a algunas de estas secuencias de comandos con más detalle.

## <a name="models"></a>Modelos

Los modelos se definen en la carpeta Scripts/aplicación/modelos. Hay dos archivos de modelo: todoItem.js y todoList.js.

**todo.Model.js** define los modelos del lado cliente (explorador) para las listas de tareas pendientes. Hay dos clases de modelo: todoItem y todoList. En Ember, los modelos son subclases de DS. Modelo. Un modelo puede tener propiedades con atributos:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Los modelos pueden definir relaciones con otros modelos de:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Los modelos pueden han calculado las propiedades que se enlazan a otras propiedades:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Los modelos pueden tener funciones de observador, que se invocan cuando cambia una propiedad observada:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Vistas

Las vistas se definen en la carpeta vistas/app/secuencias de comandos. Eventos del interfaz de usuario de la aplicación traduce en una vista. Un controlador de eventos puede devolver la llamada a funciones de controlador, o simplemente llamar directamente al contexto de datos.

Por ejemplo, el código siguiente procede de views/TodoItemEditView.js. Define el control de eventos para un campo de texto de entrada.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controlador

Los controladores se definen en la carpeta de Scripts, aplicaciones o controladores. Para representar un único modelo, extender `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Un controlador también puede representar una colección de modelos mediante la extensión `Ember.ArrayController`. Por ejemplo, el TodoListController representa una matriz de `todoList` objetos. El controlador se ordena por identificador de todoList, en orden descendente:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

El controlador define una función denominada `addTodoList`, que crea un nuevo todoList y lo agrega a la matriz. Para ver cómo obtiene llama a esta función, abra el archivo de plantilla denominado todoListTemplate.html, en la carpeta de plantillas. El siguiente código de plantilla enlaza a un botón a la `addTodoList` función:

[!code-html[Main](emberjs-template/samples/sample8.html)]

El controlador también contiene un `error` propiedad, que contiene un mensaje de error. Este es el código de plantilla para mostrar el mensaje de error (también en todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Rutas

Router.js define las rutas y la plantilla predeterminada para mostrar, configura el estado de la aplicación y coincide con las direcciones URL a las rutas:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js carga los datos para el TodoListRoute invalidando la función setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember emplea convenciones de nomenclatura para que coincida con las direcciones URL, los nombres de ruta, controladores y las plantillas. Para obtener más información, consulte [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) en la documentación de EmberJS.

## <a name="templates"></a>Plantillas

La carpeta de plantillas contiene cuatro plantillas:

- Application.HBS: la plantilla predeterminada que se representa cuando se inicia la aplicación.
- About.HBS: la plantilla para la ruta "/ aproximadamente".
- index.HBS: la plantilla para la raíz de la ruta "/".
- todoList.hbs: la plantilla para la "/ lista de tareas" ruta.
- \_NavBar.HBS: la plantilla define el menú de navegación.

La plantilla de aplicación actúa como una página maestra. Contiene un encabezado, un pie de página y un "{{outlet}}" para insertar otras plantillas en función de la ruta. Para obtener más información acerca de las plantillas de aplicación en Ember, consulte [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

El "/ todoList" plantilla contiene dos expresiones del bucle. El bucle exterior es `{{#each controller}}`y el interior del bucle es `{{#each todos}}`. El código siguiente muestra un integrado `Ember.Checkbox` ver una personalizada `App.TodoItemEditView`y un vínculo con un `deleteTodo` acción.

[!code-html[Main](emberjs-template/samples/sample12.html)]

El `HtmlHelperExtensions` clase, definida en Controllers/HtmlHelperExensions.cs, define una aplicación auxiliar de función para almacenar en caché e Insertar plantilla archivos al **depurar** está establecido en **true** en el archivo Web.config. Esta función se llama desde el archivo de vista de MVC de ASP.NET definido en Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Se llama sin argumentos, la función representa todos los archivos de plantilla en la carpeta de plantillas. También puede especificar una subcarpeta o un archivo de plantilla específica.

Cuando **depurar** es **false** en Web.config, la aplicación incluye el elemento de paquete "~/bundles/templates". Se agrega este elemento de agrupación de BundleConfig.cs, mediante la biblioteca de Handlebars del compilador:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
