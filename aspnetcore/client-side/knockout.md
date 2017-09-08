---
title: Marco de knockout.js MVVM en ASP.NET Core
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: 87b4fdc86f6bb870ae0a8cc85688a549fd0740ac
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="2c4cb-103">Marco de knockout.js MVVM en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c4cb-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="2c4cb-104">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="2c4cb-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="2c4cb-105">Knockout es una biblioteca de JavaScript popular que simplifica la creación de interfaces de usuario complejas de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="2c4cb-106">Se puede utilizar por sí solo o con otras bibliotecas, como jQuery.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="2c4cb-107">Su objetivo principal es enlazar elementos de interfaz de usuario para un modelo de datos subyacente que se define como un objeto de JavaScript, de forma que cuando se realizan cambios en la interfaz de usuario, se actualiza el modelo y viceversa.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="2c4cb-108">Knockout facilita el uso de un modelo Model-View-ViewModel (MVVM) en el comportamiento del lado cliente de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="2c4cb-109">Los dos conceptos principales uno necesario saber cuando se trabaja con la implementación de MVVM del Knockout son objetos Observables y enlaces.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2c4cb-110">Introducción</span><span class="sxs-lookup"><span data-stu-id="2c4cb-110">Getting started</span></span>

<span data-ttu-id="2c4cb-111">Knockout se implementa como un solo archivo de JavaScript, por lo que la instalación y uso es muy sencillo con [bower](bower.md).</span><span class="sxs-lookup"><span data-stu-id="2c4cb-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="2c4cb-112">Suponiendo que ya tenga [bower](bower.md) y [gulp](using-gulp.md) configurado, abra *bower.json* en el núcleo de ASP.NET del proyecto y agrega la dependencia de knockout como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

<span data-ttu-id="2c4cb-113">Con esto en su lugar, puede después manualmente ejecuta bower, abra el explorador del ejecutor de tareas (en vista ‣ otras ventanas ‣ Explorador del ejecutor de tareas) y, a continuación, en tareas, haga doble clic en bower y seleccione Ejecutar.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="2c4cb-114">El resultado debería ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-114">The result should appear similar to this:</span></span>

![bower cobertura de ejecución en el explorador del ejecutor de tareas](knockout/_static/bower-knockout.png)

<span data-ttu-id="2c4cb-116">Ahora, si mira en el proyecto `wwwroot` carpeta, debería ver knockout instalado en la carpeta "lib".</span><span class="sxs-lookup"><span data-stu-id="2c4cb-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![knockout instalado en la carpeta "lib"](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="2c4cb-118">Se recomienda que en su entorno de producción referencia knockout a través de una red de entrega de contenido o de red CDN, a medida que esto aumenta la probabilidad de que los usuarios ya tendrán una copia en caché del archivo y, por tanto, no tendrá que descargarlo en absoluto.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="2c4cb-119">Knockout está disponible en varias redes CDN, incluida la CDN de Microsoft Ajax, aquí:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="2c4cb-120">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="2c4cb-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="2c4cb-121">Para incluir Knockout en una página que va a usar, basta con agregar un `<script>` elemento hace referencia al archivo desde donde se va a hospedarlo (con la aplicación o a través de una CDN):</span><span class="sxs-lookup"><span data-stu-id="2c4cb-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="2c4cb-122">Observables y ViewModels, enlace simple</span><span class="sxs-lookup"><span data-stu-id="2c4cb-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="2c4cb-123">Puede que ya esté familiarizado con utiliza JavaScript para manipular los elementos en una página web, ya sea a través del acceso directo al DOM o utilizar una biblioteca como jQuery.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="2c4cb-124">Normalmente, este tipo de comportamiento se logra mediante la escritura de código para establecer directamente los valores de elemento en respuesta a alguna acción del usuario.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="2c4cb-125">Con Knockout, un enfoque declarativo se realiza en su lugar, a través del cual los elementos de la página se enlazan a propiedades en un objeto.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="2c4cb-126">En lugar de escribir código para manipular los elementos DOM, las acciones del usuario simplemente interactúan con el objeto ViewModel y Knockout se encarga de asegurarse de que se sincronizan los elementos de la página.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="2c4cb-127">Como ejemplo sencillo, considere la siguiente lista de página.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="2c4cb-128">Incluye un `<span>` elemento con un `data-bind` atributo que indica que se debe enlazar el contenido de texto Nombre del autor.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="2c4cb-129">Después, en un bloque de JavaScript se define un variable viewModel con una propiedad única, `authorName`, establecido a algún valor.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="2c4cb-130">Por último, una llamada a `ko.applyBindings` se realiza, pasando esta variable viewModel.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

<span data-ttu-id="2c4cb-131">Cuando se ve en el explorador, el contenido de la <span> elemento se reemplaza con el valor de la variable de modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![enlace simple knockout](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="2c4cb-133">Ahora tenemos trabajo simple enlace unidireccional.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="2c4cb-134">Tenga en cuenta que ninguna parte en el código se escribimos JavaScript para asignar un valor para el contenido del intervalo.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="2c4cb-135">Si desea manipular el modelo de vista, podemos realizar esto un paso más y agregar un cuadro de texto de entrada de HTML y enlazar a su valor, como para:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="2c4cb-136">Volver a cargar la página, vemos que este valor se enlaza realmente en el cuadro de entrada:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![enlace de entrada de cobertura](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="2c4cb-138">Sin embargo, si se cambia el valor en el cuadro de texto, el valor correspondiente de la `<span>` no cambia el elemento.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="2c4cb-139">¿A qué se debe?</span><span class="sxs-lookup"><span data-stu-id="2c4cb-139">Why not?</span></span>

<span data-ttu-id="2c4cb-140">El problema es que no hay nada notifique el `<span>` que necesitan actualizarse.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="2c4cb-141">Simplemente se actualiza el modelo de vista no es por sí mismo suficiente, a menos que las propiedades del modelo de vista se encapsulan en un tipo especial.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="2c4cb-142">Que debemos usar **observables** en el modelo de vista para todas las propiedades que necesitan tener los cambios se actualizan automáticamente cuando se producen.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="2c4cb-143">Si cambia el modelo de vista para usar `ko.observable("value")` en lugar de simplemente "value", el modelo de vista actualizará los elementos HTML que están enlazados a su valor cada vez que se produce un cambio.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="2c4cb-144">Tenga en cuenta que los cuadros de entrada no se actualizan su valor hasta que perderá el foco, por lo que no verá los cambios en elementos enlazados a medida que escribe.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="2c4cb-145">Agregar compatibilidad con la actualización dinámica después de cada keypress es simplemente cuestión de agregar `valueUpdate: "afterkeydown"` a la `data-bind` contenido del atributo.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="2c4cb-146">También puede obtener este comportamiento mediante `data-bind="textInput: authorName"` obtener las actualizaciones de instantáneas de valores.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="2c4cb-147">Nuestro modelo de vista, después de actualizar para usar ko.observable:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="2c4cb-148">Knockout admite un número de tipos diferentes de enlaces.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="2c4cb-149">Hasta ahora hemos visto cómo enlazar a `text` y a `value`.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="2c4cb-150">También puede enlazar a cualquier atributo determinado.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="2c4cb-151">Por ejemplo, para crear un hipervínculo con una etiqueta delimitadora, la `src` atributo puede estar limitado por el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="2c4cb-152">Knockout también admite el enlace a las funciones.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="2c4cb-153">Para demostrar esto, vamos a actualizar el modelo de vista para incluir el identificador de twitter del autor y mostrar el identificador de twitter como un vínculo a la página de twitter del autor.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="2c4cb-154">Deberá hacerlo en tres fases.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-154">We'll do this in three stages.</span></span>

<span data-ttu-id="2c4cb-155">En primer lugar, agregue el código HTML para mostrar el hipervínculo, lo que le mostraremos entre paréntesis después el nombre del autor:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="2c4cb-156">A continuación, actualice el modelo de vista para incluir las propiedades twitterUrl y twitterAlias:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="2c4cb-157">Tenga en cuenta que en este momento se aún no hemos actualizado el twitterUrl para ir a la dirección URL correcta para este alias de twitter, que apunta justo en twitter.com.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com.</span></span> <span data-ttu-id="2c4cb-158">Observe también que estamos usando una nueva función de cobertura, `computed`, para twitterUrl.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-158">Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="2c4cb-159">Se trata de una función observable que notifiquen a los elementos de interfaz de usuario si se cambia.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-159">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="2c4cb-160">Sin embargo, para que pueda tener acceso a otras propiedades en el modelo de vista, es necesario cambiar cómo vamos a crear el modelo de vista, para que cada propiedad es su propia instrucción.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-160">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="2c4cb-161">A continuación se muestra la declaración de viewModel revisada.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-161">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="2c4cb-162">Ahora se declara como una función.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-162">It is now declared as a function.</span></span> <span data-ttu-id="2c4cb-163">Tenga en cuenta que cada propiedad es ahora, su propia instrucción termina con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-163">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="2c4cb-164">Tenga en cuenta que para tener acceso al valor de propiedad de twitterAlias, es necesario ejecutarlo, su referencia incluye ().</span><span class="sxs-lookup"><span data-stu-id="2c4cb-164">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="2c4cb-165">El resultado funciona según lo previsto en el explorador:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-165">The result works as expected in the browser:</span></span>

![hipervínculo de cobertura](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="2c4cb-167">Knockout también admite el enlace a determinados eventos de elemento de interfaz de usuario, como el evento de clic.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-167">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="2c4cb-168">Esto le permite enlazar mediante declaración y fácilmente elementos de interfaz de usuario a funciones en el modelo de vista de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-168">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="2c4cb-169">Como ejemplo sencillo, podemos agregar un botón que, al hacer clic, modifica twitterAlias del autor para que sea todo en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-169">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="2c4cb-170">En primer lugar, agregamos el botón, haga clic en el enlace para el botón eventos y haciendo referencia al nombre de función que vamos a agregar para el modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-170">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="2c4cb-171">A continuación, agregue la función en el modelo de vista y conectarlo para modificar el estado del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-171">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="2c4cb-172">Tenga en cuenta que para establecer un nuevo valor a la propiedad twitterAlias, se llama como un método y pase el valor nuevo.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-172">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="2c4cb-173">La ejecución de código y haga clic en el botón modifica el vínculo que aparece como se esperaba:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-173">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![poner en mayúsculas hipervínculo](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="2c4cb-175">Flujo de control</span><span class="sxs-lookup"><span data-stu-id="2c4cb-175">Control flow</span></span>

<span data-ttu-id="2c4cb-176">Knockout incluye enlaces que pueden realizar operaciones condicionales y bucles.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-176">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="2c4cb-177">Operaciones de bucle son especialmente útiles para listas de datos de enlace a listas, los menús y cuadrículas o tablas de interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-177">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="2c4cb-178">El enlace de foreach iterará sobre una matriz.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-178">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="2c4cb-179">Cuando se utiliza con una matriz observable, se actualizará automáticamente los elementos de interfaz de usuario cuando los elementos se agregan o quitan de la matriz, sin necesidad de volver a crear todos los elementos en el árbol de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-179">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="2c4cb-180">En el ejemplo siguiente se usa un viewModel nuevo que incluye una matriz de resultados del juego observable.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-180">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="2c4cb-181">Está enlazada a una tabla simple con dos columnas con un `foreach` de enlace de la `<tbody>` elemento.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-181">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="2c4cb-182">Cada `<tr>` elemento dentro de `<tbody>` se enlazará a un elemento de la colección de gameResults.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-182">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

<span data-ttu-id="2c4cb-183">Tenga en cuenta que esta vez usamos ViewModel con una letra mayúscula "V" porque se espera construir con "new" (en la llamada applyBindings).</span><span class="sxs-lookup"><span data-stu-id="2c4cb-183">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="2c4cb-184">Cuando se ejecuta, la página genera el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-184">When executed, the page results in the following output:</span></span>

![modelo de vista de registros de cobertura](knockout/_static/record-screenshot.png)

<span data-ttu-id="2c4cb-186">Para demostrar que la colección observable funciona, vamos a agregar un poco más funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-186">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="2c4cb-187">Podemos incluyen la capacidad de registrar los resultados de otro juego para el modelo de vista y, a continuación, agregue un botón y alguna interfaz de usuario para trabajar con esta nueva función.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-187">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="2c4cb-188">En primer lugar, vamos a crear el método addResult:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-188">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="2c4cb-189">Este método de enlace en un botón mediante la `click` enlace:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-189">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="2c4cb-190">Abra la página en el explorador y haga clic en el botón de un par de veces, lo que da lugar a una nueva fila de tabla con cada clic:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-190">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![Agregar resultados](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="2c4cb-192">Hay varias maneras para admitir la incorporación de nuevos registros en la interfaz de usuario, por lo general, ya sea en línea o en un formulario independiente.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-192">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="2c4cb-193">Se podrá modificar fácilmente la tabla para usar los cuadros de texto y listas desplegables para que todo el material es editable.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-193">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="2c4cb-194">Sólo debe cambiar la `<tr>` elemento tal como se muestra:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-194">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="2c4cb-195">Tenga en cuenta que `$root` hace referencia a la raíz del modelo de vista, que es donde se exponen las opciones posibles.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-195">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="2c4cb-196">`$data`hace referencia a cualquier el modelo actual está dentro de un contexto determinado: en este caso, que hace referencia a un elemento individual de la matriz resultChoices, cada uno de los cuales es una cadena simple.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-196">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="2c4cb-197">Con este cambio, la cuadrícula completa se convierte en modificable:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-197">With this change, the entire grid becomes editable:</span></span>

![Cuadrícula editable](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="2c4cb-199">Si no estuviéramos utilizando Knockout, todo esto puede lograr mediante jQuery, pero probablemente no podría ser casi tan eficaz.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-199">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="2c4cb-200">Knockout realiza un seguimiento de los datos enlazados los elementos en el modelo de vista se corresponden a los elementos de interfaz de usuario y sólo actualiza los elementos que necesitan ser agregado, eliminado o actualizado.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-200">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="2c4cb-201">Tardaría esfuerzo significativo para lograr esto desarrollamos mediante jQuery o la manipulación directa de DOM, y aún así si, a continuación, deseamos mostrar resultados agregados (por ejemplo, un registro win-pérdida) basados en datos de la tabla, es necesario crear un bucle a través de él una vez más y analizar el Elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-201">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="2c4cb-202">Con Knockout, mostrar el registro win-pérdida es trivial.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-202">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="2c4cb-203">Podemos realizar los cálculos en el modelo de vista y, a continuación, se muestran con un enlace de texto simple y un `<span>`.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-203">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="2c4cb-204">Para compilar la cadena de registro de pérdida de win, podemos usar un objeto observable calculada.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-204">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="2c4cb-205">Tenga en cuenta que las referencias a propiedades observables en el modelo de vista debe ser llamadas a funciones, en caso contrario no recuperará el valor de la observable (es decir, `gameResults()` no `gameResults` en el código se muestra):</span><span class="sxs-lookup"><span data-stu-id="2c4cb-205">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="2c4cb-206">Enlazar esta función en un intervalo dentro de la `<h1>` elemento situado al principio de la página:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-206">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="2c4cb-207">El resultado:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-207">The result:</span></span>

![Pérdida de Win](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="2c4cb-209">Agregar filas o modificar el elemento seleccionado en la columna de resultados de la fila se actualizará el registro que se muestra en la parte superior de la ventana.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-209">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="2c4cb-210">Además de enlace en valores, también puede utilizar casi cualquier expresión de JavaScript legal dentro de un enlace.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-210">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="2c4cb-211">Por ejemplo, si un elemento de interfaz de usuario solo debería aparecer bajo ciertas condiciones, por ejemplo, cuando un valor supera un cierto umbral, puede especificarlo lógicamente dentro de la expresión de enlace:</span><span class="sxs-lookup"><span data-stu-id="2c4cb-211">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="2c4cb-212">Esto `<div>` sólo estará visible cuando la customerValue es superiores a 100.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-212">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="2c4cb-213">Plantillas</span><span class="sxs-lookup"><span data-stu-id="2c4cb-213">Templates</span></span>

<span data-ttu-id="2c4cb-214">Knockout es compatible con las plantillas, para que puedan separar fácilmente la interfaz de usuario de su comportamiento, o bien cargar incrementalmente los elementos de interfaz de usuario en una aplicación de gran tamaño a petición.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-214">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="2c4cb-215">Podemos actualizar nuestro ejemplo anterior para hacer que cada fila su propia plantilla simplemente extrae el código HTML horizontal en una plantilla y especificando la plantilla por su nombre en la llamada de enlace de datos en `<tbody>`.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-215">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

<span data-ttu-id="2c4cb-216">Knockout también admite otros motores de plantillas, como la biblioteca de jQuery.tmpl y motor de plantillas de Underscore.js.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-216">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="2c4cb-217">Componentes</span><span class="sxs-lookup"><span data-stu-id="2c4cb-217">Components</span></span>

<span data-ttu-id="2c4cb-218">Componentes le permiten organizar y reutilizar código de interfaz de usuario, normalmente junto con los datos de modelo de vista que depende el código de interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-218">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="2c4cb-219">Para crear un componente, basta con especificar la plantilla y su modelo de vista y asígnele un nombre.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-219">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="2c4cb-220">Esto se hace llamando a `ko.components.register()`.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-220">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="2c4cb-221">Además de definir las plantillas y viewmodel alineado, se pueden cargar desde un archivo externo con una biblioteca como *require.js*, da lugar a un código muy limpio y eficaz.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-221">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="2c4cb-222">Comunicarse con las API</span><span class="sxs-lookup"><span data-stu-id="2c4cb-222">Communicating with APIs</span></span>

<span data-ttu-id="2c4cb-223">Knockout puede trabajar con los datos en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-223">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="2c4cb-224">Una forma habitual para recuperar y guardar datos usando Knockout es con jQuery, que admite la `$.getJSON()` función para recuperar datos y el `$.post()` método para enviar datos desde el explorador a un punto de conexión de API.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-224">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="2c4cb-225">Por supuesto, si lo prefiere de forma diferente para enviar y recibir datos JSON, Knockout funcionará con él así.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-225">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="2c4cb-226">Resumen</span><span class="sxs-lookup"><span data-stu-id="2c4cb-226">Summary</span></span>

<span data-ttu-id="2c4cb-227">Knockout proporciona una manera sencilla y elegante para enlazar los elementos de interfaz de usuario para el estado actual de la aplicación de cliente, que se definen en un modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-227">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="2c4cb-228">Sintaxis de enlace de knockout utiliza el atributo de enlace de datos, aplicado a elementos HTML que se procesan.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-228">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="2c4cb-229">Knockout es capaz de procesar eficazmente y actualizar grandes conjuntos de datos mediante el seguimiento de elementos de interfaz de usuario y sólo procesando cambios en afectados los elementos.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-229">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="2c4cb-230">Aplicaciones de gran tamaño pueden interrumpir la lógica de la interfaz de usuario mediante plantillas y componentes, que se pueden cargar a petición desde un archivo externo.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-230">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="2c4cb-231">Actualmente versión 3, Knockout es una biblioteca de JavaScript estable que puede mejorar las aplicaciones web que requieren la interactividad de cliente enriquecido.</span><span class="sxs-lookup"><span data-stu-id="2c4cb-231">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>
