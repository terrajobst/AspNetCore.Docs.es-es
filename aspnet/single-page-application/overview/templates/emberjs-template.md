---
uid: single-page-application/overview/templates/emberjs-template
title: Plantilla de EmberJS | Microsoft Docs
author: xqiu
description: Plantilla de EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: fbc3b1d299ace27d38d895e42b8e3bb3b51b36f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836598"
---
<a name="emberjs-template"></a><span data-ttu-id="5fa4f-103">Plantilla de EmberJS</span><span class="sxs-lookup"><span data-stu-id="5fa4f-103">EmberJS template</span></span>
====================
<span data-ttu-id="5fa4f-104">por [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="5fa4f-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="5fa4f-105">La plantilla de EmberJS MVC está escrita por Nathan Totten, Thiago Santos y Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="5fa4f-106">Descargue la plantilla de EmberJS MVC</span><span class="sxs-lookup"><span data-stu-id="5fa4f-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="5fa4f-107">La plantilla de EmberJS SPA está diseñada para ayudarle a comenzar a crear rápidamente aplicaciones web interactivas del lado cliente mediante EmberJS.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="5fa4f-108">"Aplicación de página única" (SPA) es el término general para una aplicación web que se carga una página HTML única y, a continuación, actualiza la página de forma dinámica, en lugar de cargar páginas nuevas.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="5fa4f-109">Después de la carga de página inicial, la SPA se comunica con el servidor a través de las solicitudes AJAX.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="5fa4f-110">AJAX no es nada nuevo, pero hoy en día existen marcos de JavaScript que resulte más fácil crear y mantener una gran aplicación SPA sofisticada.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="5fa4f-111">Además, HTML5 y CSS3 son más fácil crear interfaces de usuario enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="5fa4f-112">La plantilla de EmberJS SPA usa el [Ember](http://emberjs.com/) biblioteca de JavaScript para controlar las actualizaciones de la página de las solicitudes AJAX.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="5fa4f-113">Ember.js usa el enlace de datos para sincronizar la página con los datos más recientes.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="5fa4f-114">De este modo, no tendrá que escribir ningún código que le guía a través de los datos JSON y actualiza el DOM.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="5fa4f-115">En su lugar, coloque atributos declarativos en el código HTML que indican Ember.js cómo presentar los datos.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="5fa4f-116">En el servidor, es casi idéntica a la plantilla de EmberJS la [plantilla KnockoutJS SPA](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="5fa4f-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="5fa4f-117">ASP.NET MVC usa para atender a documentos HTML y ASP.NET Web API para controlar las solicitudes AJAX desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="5fa4f-118">Para obtener más información sobre los aspectos de la plantilla, consulte el [plantilla KnockoutJS](../introduction/knockoutjs-template.md) documentación.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="5fa4f-119">En este tema se centra en las diferencias entre la plantilla de Knockout y la plantilla de EmberJS.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="5fa4f-120">Crear un proyecto de plantilla de EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="5fa4f-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="5fa4f-121">Descargue e instale la plantilla, haga clic en el botón de descarga anterior.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="5fa4f-122">Es posible que deba reiniciar Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="5fa4f-123">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="5fa4f-124">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="5fa4f-125">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="5fa4f-126">Denomine el proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="5fa4f-127">En el **nuevo proyecto** asistente, seleccione **Ember.js SPA Project**.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="5fa4f-128">Introducción a la plantilla de EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="5fa4f-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="5fa4f-129">La plantilla de EmberJS usa una combinación de jQuery, Ember.js, Handlebars.js para crear una interfaz de usuario interactiva y sin problemas.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="5fa4f-130">Ember.js es una biblioteca de JavaScript que usa un patrón MVC del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="5fa4f-131">Un *plantilla*, escrito en el lenguaje de plantillas Handlebars, se describe la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="5fa4f-132">En modo de lanzamiento, el [compilador Handlebars](https://github.com/Myslik/csharp-ember-handlebars) se usa para agrupar y compilar la plantilla de handlebars.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="5fa4f-133">Un *modelo* almacena los datos de aplicación que obtiene del servidor (listas de tareas pendientes y elementos de lista de tareas).</span><span class="sxs-lookup"><span data-stu-id="5fa4f-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="5fa4f-134">Un *controlador* almacena el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-134">A *controller* stores application state.</span></span> <span data-ttu-id="5fa4f-135">Los controladores suelen presentan los datos de modelo a las plantillas correspondientes.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="5fa4f-136">Un *vista* traduce primitivos eventos de la aplicación y los pasa al controlador.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="5fa4f-137">Un *enrutador* administra el estado de la aplicación, lo que las direcciones URL y las plantillas mantiene sincronizados.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="5fa4f-138">Además, la biblioteca de Ember datos puede utilizarse para sincronizar objetos JSON (obtenidos desde el servidor a través de una API de REST) y los modelos de cliente.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="5fa4f-139">La plantilla de EmberJS SPA organiza las secuencias de comandos en ocho capas:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="5fa4f-140">webapi\_adapter.js, webapi\_serializer.js: amplía la biblioteca de Ember datos para que funcione con ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="5fa4f-141">Scripts/helpers.js: Define las nuevas aplicaciones auxiliares de Ember Handlebars.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="5fa4f-142">Scripts/App.js: Se crea la aplicación y se configura el adaptador y el serializador.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="5fa4f-143">Las secuencias decomandos/aplicación/modelos/\*.js: define los modelos.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="5fa4f-144">Vistas, secuencias decomandos/app/\*.js: define las vistas.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="5fa4f-145">Las secuencias decomandos/aplicación/controladores/\*.js: define los controladores.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="5fa4f-146">Las secuencias de comandos/aplicación/routes, Scripts/app/router.js: Define las rutas.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="5fa4f-147">Plantillas /\*.hbs: define las plantillas handlebars.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="5fa4f-148">Echemos un vistazo a algunas de estas secuencias de comandos con más detalle.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="5fa4f-149">Modelos</span><span class="sxs-lookup"><span data-stu-id="5fa4f-149">Models</span></span>

<span data-ttu-id="5fa4f-150">Los modelos se definen en la carpeta Scripts/aplicación/modelos.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="5fa4f-151">Hay dos archivos de modelo: todoItem.js y todoList.js.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="5fa4f-152">**todo.Model.js** define los modelos del lado cliente (explorador) para las listas de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="5fa4f-153">Hay dos clases de modelo: todoItem y todoList.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="5fa4f-154">En Ember, los modelos son subclases de DS. Modelo.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="5fa4f-155">Un modelo puede tener propiedades con atributos:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="5fa4f-156">Los modelos pueden definir relaciones con otros modelos de:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="5fa4f-157">Los modelos pueden han calculado las propiedades que se enlazan a otras propiedades:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="5fa4f-158">Los modelos pueden tener funciones de observador, que se invocan cuando cambia una propiedad observada:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="5fa4f-159">Vistas</span><span class="sxs-lookup"><span data-stu-id="5fa4f-159">Views</span></span>

<span data-ttu-id="5fa4f-160">Las vistas se definen en la carpeta vistas/app/secuencias de comandos.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="5fa4f-161">Eventos del interfaz de usuario de la aplicación traduce en una vista.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-161">A view translates events from the application UI.</span></span> <span data-ttu-id="5fa4f-162">Un controlador de eventos puede devolver la llamada a funciones de controlador, o simplemente llamar directamente al contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="5fa4f-163">Por ejemplo, el código siguiente procede de views/TodoItemEditView.js.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="5fa4f-164">Define el control de eventos para un campo de texto de entrada.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="5fa4f-165">Controlador</span><span class="sxs-lookup"><span data-stu-id="5fa4f-165">Controller</span></span>

<span data-ttu-id="5fa4f-166">Los controladores se definen en la carpeta de Scripts, aplicaciones o controladores.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="5fa4f-167">Para representar un único modelo, extender `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="5fa4f-168">Un controlador también puede representar una colección de modelos mediante la extensión `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="5fa4f-169">Por ejemplo, el TodoListController representa una matriz de `todoList` objetos.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="5fa4f-170">El controlador se ordena por identificador de todoList, en orden descendente:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="5fa4f-171">El controlador define una función denominada `addTodoList`, que crea un nuevo todoList y lo agrega a la matriz.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="5fa4f-172">Para ver cómo obtiene llama a esta función, abra el archivo de plantilla denominado todoListTemplate.html, en la carpeta de plantillas.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="5fa4f-173">El siguiente código de plantilla enlaza a un botón a la `addTodoList` función:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="5fa4f-174">El controlador también contiene un `error` propiedad, que contiene un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="5fa4f-175">Este es el código de plantilla para mostrar el mensaje de error (también en todoListTemplate.html):</span><span class="sxs-lookup"><span data-stu-id="5fa4f-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="5fa4f-176">Rutas</span><span class="sxs-lookup"><span data-stu-id="5fa4f-176">Routes</span></span>

<span data-ttu-id="5fa4f-177">Router.js define las rutas y la plantilla predeterminada para mostrar, configura el estado de la aplicación y coincide con las direcciones URL a las rutas:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="5fa4f-178">TodoListRoute.js carga los datos para el TodoListRoute invalidando la función setupController:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="5fa4f-179">Ember emplea convenciones de nomenclatura para que coincida con las direcciones URL, los nombres de ruta, controladores y las plantillas.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="5fa4f-180">Para obtener más información, consulte [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) en la documentación de EmberJS.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="5fa4f-181">Plantillas</span><span class="sxs-lookup"><span data-stu-id="5fa4f-181">Templates</span></span>

<span data-ttu-id="5fa4f-182">La carpeta de plantillas contiene cuatro plantillas:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="5fa4f-183">Application.HBS: la plantilla predeterminada que se representa cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="5fa4f-184">About.HBS: la plantilla para la ruta "/ aproximadamente".</span><span class="sxs-lookup"><span data-stu-id="5fa4f-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="5fa4f-185">index.HBS: la plantilla para la raíz de la ruta "/".</span><span class="sxs-lookup"><span data-stu-id="5fa4f-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="5fa4f-186">todoList.hbs: la plantilla para la "/ lista de tareas" ruta.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="5fa4f-187">\_NavBar.HBS: la plantilla define el menú de navegación.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="5fa4f-188">La plantilla de aplicación actúa como una página maestra.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-188">The application template acts like a master page.</span></span> <span data-ttu-id="5fa4f-189">Contiene un encabezado, un pie de página y un "{{outlet}}" para insertar otras plantillas en función de la ruta.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="5fa4f-190">Para obtener más información acerca de las plantillas de aplicación en Ember, consulte [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="5fa4f-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="5fa4f-191">El "/ todoList" plantilla contiene dos expresiones del bucle.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="5fa4f-192">El bucle exterior es `{{#each controller}}`y el interior del bucle es `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="5fa4f-193">El código siguiente muestra un integrado `Ember.Checkbox` ver una personalizada `App.TodoItemEditView`y un vínculo con un `deleteTodo` acción.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="5fa4f-194">El `HtmlHelperExtensions` clase, definida en Controllers/HtmlHelperExensions.cs, define una aplicación auxiliar de función para almacenar en caché e Insertar plantilla archivos al **depurar** está establecido en **true** en el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="5fa4f-195">Esta función se llama desde el archivo de vista de MVC de ASP.NET definido en Views/Home/App.cshtml:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="5fa4f-196">Se llama sin argumentos, la función representa todos los archivos de plantilla en la carpeta de plantillas.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="5fa4f-197">También puede especificar una subcarpeta o un archivo de plantilla específica.</span><span class="sxs-lookup"><span data-stu-id="5fa4f-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="5fa4f-198">Cuando **depurar** es **false** en Web.config, la aplicación incluye el elemento de paquete "~/bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="5fa4f-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="5fa4f-199">Se agrega este elemento de agrupación de BundleConfig.cs, mediante la biblioteca de Handlebars del compilador:</span><span class="sxs-lookup"><span data-stu-id="5fa4f-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
