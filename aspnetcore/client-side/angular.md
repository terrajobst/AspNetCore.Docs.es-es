---
title: "Uso de AngularJS para aplicaciones de una página (SPAs)"
author: rick-anderson
description: "Obtenga información acerca de cómo crear una aplicación de ASP.NET SPA estilo con AngularJS"
keywords: "Núcleo de ASP.NET, AngularJS, SPA"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4aecf9e9bd11cc7e2b36b40955178d9e9368c185
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a><span data-ttu-id="ef93a-104">Uso de AngularJS para aplicaciones de una página (SPAs) con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef93a-104">Using AngularJS for Single Page Applications (SPAs) with ASP.NET Core</span></span>


<span data-ttu-id="ef93a-105">Por [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="ef93a-105">By [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="ef93a-106">En este artículo, aprenderá cómo crear una aplicación de ASP.NET SPA estilo con AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-106">In this article, you will learn how to build a SPA-style ASP.NET application using AngularJS.</span></span>

<span data-ttu-id="ef93a-107">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef93a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-angularjs"></a><span data-ttu-id="ef93a-108">¿Qué es AngularJS?</span><span class="sxs-lookup"><span data-stu-id="ef93a-108">What is AngularJS?</span></span>

<span data-ttu-id="ef93a-109">[AngularJS](https://angularjs.org/) es un marco de JavaScript moderno de Google que se usa normalmente para trabajar con aplicaciones de una sola página (SPAs).</span><span class="sxs-lookup"><span data-stu-id="ef93a-109">[AngularJS](https://angularjs.org/) is a modern JavaScript framework from Google commonly used to work with Single Page Applications (SPAs).</span></span> <span data-ttu-id="ef93a-110">AngularJS está abierto con origen bajo licencia MIT y puede seguir el progreso de la programación de AngularJS [su repositorio de GitHub](https://github.com/angular/angular.js).</span><span class="sxs-lookup"><span data-stu-id="ef93a-110">AngularJS is open sourced under MIT license, and the development progress of AngularJS can be followed on [its GitHub repository](https://github.com/angular/angular.js).</span></span> <span data-ttu-id="ef93a-111">La biblioteca se denomina Angular porque HTML utiliza corchetes angulares en forma.</span><span class="sxs-lookup"><span data-stu-id="ef93a-111">The library is called Angular because HTML uses angular-shaped brackets.</span></span>

<span data-ttu-id="ef93a-112">AngularJS no es una biblioteca de manipulación de DOM como jQuery, pero utiliza un subconjunto de jQuery denominado jQLite.</span><span class="sxs-lookup"><span data-stu-id="ef93a-112">AngularJS is not a DOM manipulation library like jQuery, but it uses a subset of jQuery called jQLite.</span></span> <span data-ttu-id="ef93a-113">AngularJS se basa principalmente en los atributos declarativos de HTML que se pueden agregar a las etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="ef93a-113">AngularJS is primarily based on declarative HTML attributes that you can add to your HTML tags.</span></span> <span data-ttu-id="ef93a-114">Puede intentar AngularJS en el explorador mediante la [sitio Web de código School](https://www.codeschool.com/courses/shaping-up-with-angularjs) o [sitio Web de W3Schools](https://www.w3schools.com/angular/).</span><span class="sxs-lookup"><span data-stu-id="ef93a-114">You can try AngularJS in your browser using the [Code School website](https://www.codeschool.com/courses/shaping-up-with-angularjs) or  [W3Schools website](https://www.w3schools.com/angular/).</span></span>

<span data-ttu-id="ef93a-115">En este artículo se centra en AngularJS con algunas notas en donde se dirige Angular.</span><span class="sxs-lookup"><span data-stu-id="ef93a-115">This article focuses on AngularJS with some notes on where Angular is heading.</span></span>

## <a name="getting-started"></a><span data-ttu-id="ef93a-116">Introducción</span><span class="sxs-lookup"><span data-stu-id="ef93a-116">Getting started</span></span>

<span data-ttu-id="ef93a-117">Para empezar a usar AngularJS en la aplicación ASP.NET, debe instalar como parte de su proyecto o hacer referencia a él desde una red de entrega de contenido (CDN).</span><span class="sxs-lookup"><span data-stu-id="ef93a-117">To start using AngularJS in your ASP.NET application, you must either install it as part of your project, or reference it from a content delivery network (CDN).</span></span>

### <a name="installation"></a><span data-ttu-id="ef93a-118">Instalación</span><span class="sxs-lookup"><span data-stu-id="ef93a-118">Installation</span></span>

<span data-ttu-id="ef93a-119">Hay varias maneras de agregar AngularJS a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef93a-119">There are several ways to add AngularJS to your application.</span></span> <span data-ttu-id="ef93a-120">Si está iniciando una nueva aplicación web de ASP.NET Core en Visual Studio, puede agregar AngularJS mediante integrado [Bower](bower.md) admite.</span><span class="sxs-lookup"><span data-stu-id="ef93a-120">If you’re starting a new ASP.NET Core web application in Visual Studio, you can add AngularJS using the built-in [Bower](bower.md) support.</span></span> <span data-ttu-id="ef93a-121">Abra *bower.json*y agregar una entrada a la `dependencies` propiedad:</span><span class="sxs-lookup"><span data-stu-id="ef93a-121">Open *bower.json*, and add an entry to the `dependencies` property:</span></span>

<a name=angular-bower-json></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

<span data-ttu-id="ef93a-122">Al guardar la *bower.json* archivo, Angular se instalará en el proyecto *wwwroot/lib* carpeta.</span><span class="sxs-lookup"><span data-stu-id="ef93a-122">Upon saving the *bower.json* file, Angular will be installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="ef93a-123">Además, se mostrarán en el `Dependencies/Bower` carpeta.</span><span class="sxs-lookup"><span data-stu-id="ef93a-123">Additionally, it will be listed within the `Dependencies/Bower` folder.</span></span> <span data-ttu-id="ef93a-124">Consulte la captura de pantalla siguiente.</span><span class="sxs-lookup"><span data-stu-id="ef93a-124">See the screenshot below.</span></span>

![Explorador de soluciones con proyectos de AngularJS](angular/_static/angular-solution-explorer.png)

<span data-ttu-id="ef93a-126">A continuación, agregue un `<script>` referencia a la parte inferior de la `<body>` sección de la página HTML o *_Layout.cshtml* de archivos, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="ef93a-126">Next, add a `<script>` reference to the bottom of the `<body>` section of your HTML page or *_Layout.cshtml* file, as shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

<span data-ttu-id="ef93a-127">Se recomienda que las aplicaciones de producción usan CDN para las bibliotecas comunes como AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-127">It's recommended that production applications utilize CDNs for common libraries like AngularJS.</span></span> <span data-ttu-id="ef93a-128">Puede hacer referencia a AngularJS desde uno de varios CDN, como éste:</span><span class="sxs-lookup"><span data-stu-id="ef93a-128">You can reference AngularJS from one of several CDNs, such as this one:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

<span data-ttu-id="ef93a-129">Una vez que tenga una referencia a la *angular.js* archivo de script, está listo para comenzar a usar AngularJS en las páginas web.</span><span class="sxs-lookup"><span data-stu-id="ef93a-129">Once you have a reference to the *angular.js* script file, you're ready to begin using AngularJS in your web pages.</span></span>

## <a name="key-components"></a><span data-ttu-id="ef93a-130">Componentes clave</span><span class="sxs-lookup"><span data-stu-id="ef93a-130">Key components</span></span>

<span data-ttu-id="ef93a-131">AngularJS incluye una serie de componentes principales, como *directivas*, *plantillas*, *repetidores*, *módulos*,  *controladores de*, *componentes*, *enrutador componentes* y mucho más.</span><span class="sxs-lookup"><span data-stu-id="ef93a-131">AngularJS includes a number of major components, such as *directives*, *templates*, *repeaters*, *modules*, *controllers*, *components*, *component router* and more.</span></span> <span data-ttu-id="ef93a-132">Vamos a examinar cómo funcionan juntos estos componentes para agregar un comportamiento a las páginas web.</span><span class="sxs-lookup"><span data-stu-id="ef93a-132">Let's examine how these components work together to add behavior to your web pages.</span></span>

### <a name="directives"></a><span data-ttu-id="ef93a-133">Directivas</span><span class="sxs-lookup"><span data-stu-id="ef93a-133">Directives</span></span>

<span data-ttu-id="ef93a-134">Usa AngularJS [directivas](https://docs.angularjs.org/guide/directive) extender HTML con elementos y atributos personalizados.</span><span class="sxs-lookup"><span data-stu-id="ef93a-134">AngularJS uses [directives](https://docs.angularjs.org/guide/directive) to extend HTML with custom attributes and elements.</span></span> <span data-ttu-id="ef93a-135">Directivas de AngularJS se definen a través de `data-ng-*` o `ng-*` prefijos (`ng` es una abreviación de angular).</span><span class="sxs-lookup"><span data-stu-id="ef93a-135">AngularJS directives are defined via `data-ng-*` or `ng-*` prefixes (`ng` is short for angular).</span></span> <span data-ttu-id="ef93a-136">Hay dos tipos de directivas de AngularJS:</span><span class="sxs-lookup"><span data-stu-id="ef93a-136">There are two types of AngularJS directives:</span></span>

   1. <span data-ttu-id="ef93a-137">**Directivas primitivas**: estos predefinidos por el equipo Angular y forman parte del marco AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-137">**Primitive Directives**: These are predefined by the Angular team and are part of the AngularJS framework.</span></span>

   2. <span data-ttu-id="ef93a-138">**Las directivas personalizadas de**: se trata de las directivas personalizadas que se pueden definir.</span><span class="sxs-lookup"><span data-stu-id="ef93a-138">**Custom Directives**: These are custom directives that you can define.</span></span>

<span data-ttu-id="ef93a-139">Una de las directivas de primitivas utilizadas en todas las aplicaciones de AngularJS es la `ng-app` directiva, que ejecuta la aplicación de AngularJS un bootstrap.</span><span class="sxs-lookup"><span data-stu-id="ef93a-139">One of the primitive directives used in all AngularJS applications is the `ng-app` directive, which bootstraps the AngularJS application.</span></span> <span data-ttu-id="ef93a-140">Esta directiva puede aplicarse a la `<body>` etiqueta o a un elemento secundario del cuerpo.</span><span class="sxs-lookup"><span data-stu-id="ef93a-140">This directive can be applied to the `<body>` tag or to a child element of the body.</span></span> <span data-ttu-id="ef93a-141">Veamos un ejemplo en acción.</span><span class="sxs-lookup"><span data-stu-id="ef93a-141">Let's see an example in action.</span></span> <span data-ttu-id="ef93a-142">Suponiendo que esté en un proyecto de ASP.NET, puede agregar un archivo HTML para la `wwwroot` carpeta, o agregar una nueva acción de controlador y una vista asociada.</span><span class="sxs-lookup"><span data-stu-id="ef93a-142">Assuming you're in an ASP.NET project, you can either add an HTML file to the `wwwroot` folder, or add a new controller action and an associated view.</span></span> <span data-ttu-id="ef93a-143">En este caso, he agregado un nuevo `Directives` método de acción para `HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="ef93a-143">In this case, I've added a new `Directives` action method to `HomeController.cs`.</span></span> <span data-ttu-id="ef93a-144">La vista asociada se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="ef93a-144">The associated view is shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

<span data-ttu-id="ef93a-145">Para mantener estos ejemplos independientes entre sí, no estoy usando el archivo de diseño compartido.</span><span class="sxs-lookup"><span data-stu-id="ef93a-145">To keep these samples independent of one another, I'm not using the shared layout file.</span></span> <span data-ttu-id="ef93a-146">Puede ver que es representativo de la etiqueta body con el `ng-app` directiva para indicar esta página es una aplicación de AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-146">You can see that we decorated the body tag with the `ng-app` directive to indicate this page is an AngularJS application.</span></span> <span data-ttu-id="ef93a-147">El `{{2+2}}` es una expresión de enlace de datos Angular aprenderá más acerca de en unos instantes.</span><span class="sxs-lookup"><span data-stu-id="ef93a-147">The `{{2+2}}` is an Angular data binding expression that you will learn more about in a moment.</span></span> <span data-ttu-id="ef93a-148">Este es el resultado si se ejecuta esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="ef93a-148">Here is the result if you run this application:</span></span>

![Directiva Angular simple](angular/_static/simple-directive.png)

<span data-ttu-id="ef93a-150">Incluyen otras directivas primitivos en AngularJS:</span><span class="sxs-lookup"><span data-stu-id="ef93a-150">Other primitive directives in AngularJS include:</span></span>

<span data-ttu-id="ef93a-151">`ng-controller`Determina qué controlador de JavaScript está enlazado a la vista.</span><span class="sxs-lookup"><span data-stu-id="ef93a-151">`ng-controller` Determines which JavaScript controller is bound to which view.</span></span>

<span data-ttu-id="ef93a-152">`ng-model`Determina el modelo al que se enlazan los valores de propiedades de un elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="ef93a-152">`ng-model` Determines the model to which the values of an HTML element's properties are bound.</span></span>

<span data-ttu-id="ef93a-153">`ng-init`Se utiliza para inicializar los datos de aplicación en forma de una expresión para el ámbito actual.</span><span class="sxs-lookup"><span data-stu-id="ef93a-153">`ng-init` Used to initialize the application data in the form of an expression for the current scope.</span></span>

<span data-ttu-id="ef93a-154">`ng-if`Quita o vuelve a crear el elemento HTML especificado en el DOM tomando como base el truthiness de la expresión proporcionada.</span><span class="sxs-lookup"><span data-stu-id="ef93a-154">`ng-if` Removes or recreates the given HTML element in the DOM based on the truthiness of the expression provided.</span></span>

<span data-ttu-id="ef93a-155">`ng-repeat`Repite un bloque de HTML determinado a través de un conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="ef93a-155">`ng-repeat` Repeats a given block of HTML over a set of data.</span></span>

<span data-ttu-id="ef93a-156">`ng-show`Muestra u oculta el elemento HTML especificado en función de la expresión proporcionada.</span><span class="sxs-lookup"><span data-stu-id="ef93a-156">`ng-show` Shows or hides the given HTML element based on the expression provided.</span></span>

<span data-ttu-id="ef93a-157">Para obtener una lista completa de todas las directivas de primitivas admitidos en AngularJS, consulte el [sección de documentación de la directiva en el sitio Web de documentación de AngularJS](https://docs.angularjs.org/api/ng/directive).</span><span class="sxs-lookup"><span data-stu-id="ef93a-157">For a full list of all primitive directives supported in AngularJS, please refer to the [directive documentation section on the AngularJS documentation website](https://docs.angularjs.org/api/ng/directive).</span></span>

### <a name="data-binding"></a><span data-ttu-id="ef93a-158">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="ef93a-158">Data binding</span></span>

<span data-ttu-id="ef93a-159">Proporciona AngularJS [enlace de datos](https://docs.angularjs.org/guide/databinding) admite out-of-the-box mediante la `ng-bind` directiva o una sintaxis de expresión de enlace como de datos `{{expression}}`.</span><span class="sxs-lookup"><span data-stu-id="ef93a-159">AngularJS provides [data binding](https://docs.angularjs.org/guide/databinding) support out-of-the-box using either the `ng-bind` directive or a data binding expression syntax such as `{{expression}}`.</span></span> <span data-ttu-id="ef93a-160">AngularJS admite el enlace de datos bidireccional que se conservan los datos de un modelo en la sincronización con una plantilla de vista en todo momento.</span><span class="sxs-lookup"><span data-stu-id="ef93a-160">AngularJS supports two-way data binding where data from a model is kept in synchronization with a view template at all times.</span></span> <span data-ttu-id="ef93a-161">Los cambios realizados en la vista se reflejan automáticamente en el modelo.</span><span class="sxs-lookup"><span data-stu-id="ef93a-161">Any changes to the view are automatically reflected in the model.</span></span> <span data-ttu-id="ef93a-162">Del mismo modo, los cambios en el modelo se reflejan en la vista.</span><span class="sxs-lookup"><span data-stu-id="ef93a-162">Likewise, any changes in the model are reflected in the view.</span></span>

<span data-ttu-id="ef93a-163">Crear un archivo HTML o una acción de controlador con una vista que lo acompaña denominada `Databinding`.</span><span class="sxs-lookup"><span data-stu-id="ef93a-163">Create either an HTML file or a controller action with an accompanying view named `Databinding`.</span></span> <span data-ttu-id="ef93a-164">Incluir lo siguiente en la vista:</span><span class="sxs-lookup"><span data-stu-id="ef93a-164">Include the following in the view:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

<span data-ttu-id="ef93a-165">Observe que puede mostrar los valores del modelo mediante el enlace de directivas o datos (`ng-bind`).</span><span class="sxs-lookup"><span data-stu-id="ef93a-165">Notice that you can display model values using either directives or data binding (`ng-bind`).</span></span> <span data-ttu-id="ef93a-166">La página resultante debería tener este aspecto:</span><span class="sxs-lookup"><span data-stu-id="ef93a-166">The resulting page should look like this:</span></span>

![Enlace de datos simple](angular/_static/simple-databinding.png)

### <a name="templates"></a><span data-ttu-id="ef93a-168">Plantillas</span><span class="sxs-lookup"><span data-stu-id="ef93a-168">Templates</span></span>

<span data-ttu-id="ef93a-169">[Plantillas de](https://docs.angularjs.org/guide/templates) en AngularJS páginas HTML sin formato decoradas con directivas de AngularJS y artefactos.</span><span class="sxs-lookup"><span data-stu-id="ef93a-169">[Templates](https://docs.angularjs.org/guide/templates) in AngularJS are just plain HTML pages decorated with AngularJS directives and artifacts.</span></span> <span data-ttu-id="ef93a-170">Una plantilla de AngularJS es una combinación de directivas, expresiones, filtros y controles que se combinan con HTML para la vista de formulario.</span><span class="sxs-lookup"><span data-stu-id="ef93a-170">A template in AngularJS is a mixture of directives, expressions, filters, and controls that combine with HTML to form the view.</span></span>

<span data-ttu-id="ef93a-171">Agregar otra vista para mostrar las plantillas y agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ef93a-171">Add another view to demonstrate templates, and add the following to it:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

<span data-ttu-id="ef93a-172">La plantilla tiene directivas de AngularJS como `ng-app`, `ng-init`, `ng-model` y sintaxis de expresiones de enlace de datos para enlazar el `personName` propiedad.</span><span class="sxs-lookup"><span data-stu-id="ef93a-172">The template has AngularJS directives like `ng-app`, `ng-init`, `ng-model` and data binding expression syntax to bind the `personName` property.</span></span> <span data-ttu-id="ef93a-173">Se ejecuta en el explorador, la vista es similar a la captura de pantalla siguiente:</span><span class="sxs-lookup"><span data-stu-id="ef93a-173">Running in the browser, the view looks like the screenshot below:</span></span>

![Ejemplo de plantillas simple 1](angular/_static/simple-templates-1.png)

<span data-ttu-id="ef93a-175">Si cambia el nombre, escriba en el campo de entrada, se mostrará el texto situado junto al campo de entrada dinámicamente update, que muestra el enlace de datos bidireccional Angular en acción.</span><span class="sxs-lookup"><span data-stu-id="ef93a-175">If you change the name by typing in the input field, you will see the text next to the input field dynamically update, showing Angular two-way data binding in action.</span></span>

![Ejemplo de plantillas simple 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a><span data-ttu-id="ef93a-177">Expresiones</span><span class="sxs-lookup"><span data-stu-id="ef93a-177">Expressions</span></span>

<span data-ttu-id="ef93a-178">[Expresiones](https://docs.angularjs.org/guide/expression) en AngularJS son fragmentos de código similar a JavaScript que se escriben en el `{{ expression }}` sintaxis.</span><span class="sxs-lookup"><span data-stu-id="ef93a-178">[Expressions](https://docs.angularjs.org/guide/expression) in AngularJS are JavaScript-like code snippets that are written inside the `{{ expression }}` syntax.</span></span> <span data-ttu-id="ef93a-179">Los datos de estas expresiones se enlazan a HTML del mismo modo que `ng-bind` directivas.</span><span class="sxs-lookup"><span data-stu-id="ef93a-179">The data from these expressions is bound to HTML the same way as `ng-bind` directives.</span></span> <span data-ttu-id="ef93a-180">La diferencia principal entre las expresiones regulares de JavaScript y expresiones de AngularJS es ese AngularJS expresiones se evalúan con la `$scope` objeto en AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-180">The main difference between AngularJS expressions and regular JavaScript expressions is that AngularJS expressions are evaluated against the `$scope` object in AngularJS.</span></span>

<span data-ttu-id="ef93a-181">Las expresiones de AngularJS en el ejemplo a continuación enlace `personName` y JavaScript simple calcula la expresión:</span><span class="sxs-lookup"><span data-stu-id="ef93a-181">The AngularJS expressions in the sample below bind `personName` and a simple JavaScript calculated expression:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

<span data-ttu-id="ef93a-182">En el ejemplo se ejecuta en el explorador muestra el `personName` datos y los resultados del cálculo:</span><span class="sxs-lookup"><span data-stu-id="ef93a-182">The example running in the browser displays the `personName` data and the results of the calculation:</span></span>

![Expresiones simples](angular/_static/simple-expressions.png)

### <a name="repeaters"></a><span data-ttu-id="ef93a-184">Repetidores</span><span class="sxs-lookup"><span data-stu-id="ef93a-184">Repeaters</span></span>

<span data-ttu-id="ef93a-185">Repetir en AngularJS se realiza a través de una directiva primitiva denominada `ng-repeat`.</span><span class="sxs-lookup"><span data-stu-id="ef93a-185">Repeating in AngularJS is done via a primitive directive called `ng-repeat`.</span></span> <span data-ttu-id="ef93a-186">El `ng-repeat` directiva repite un determinado elemento HTML en una vista a lo largo de una matriz de datos repetidos.</span><span class="sxs-lookup"><span data-stu-id="ef93a-186">The `ng-repeat` directive repeats a given HTML element in a view over the length of a repeating data array.</span></span> <span data-ttu-id="ef93a-187">Pueden repetir repetidores en AngularJS a través de una matriz de cadenas u objetos.</span><span class="sxs-lookup"><span data-stu-id="ef93a-187">Repeaters in AngularJS can repeat over an array of strings or objects.</span></span> <span data-ttu-id="ef93a-188">Este es un ejemplo de uso de repetición en una matriz de cadenas:</span><span class="sxs-lookup"><span data-stu-id="ef93a-188">Here is a sample usage of repeating over an array of strings:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

<span data-ttu-id="ef93a-189">El [repeat (directiva)](https://docs.angularjs.org/api/ng/directive/ngRepeat) genera una serie de elementos de lista en una lista sin ordenar, como puede ver en las herramientas de desarrollo que se muestra en esta captura de pantalla:</span><span class="sxs-lookup"><span data-stu-id="ef93a-189">The [repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat) outputs a series of list items in an unordered list, as you can see in the developer tools shown in this screenshot:</span></span>

![Ejemplo de repetidor](angular/_static/repeater.png)

<span data-ttu-id="ef93a-191">Este es un ejemplo que se repite en una matriz de objetos.</span><span class="sxs-lookup"><span data-stu-id="ef93a-191">Here is an example that repeats over an array of objects.</span></span> <span data-ttu-id="ef93a-192">El `ng-init` Directiva establece una `names` matriz, donde cada elemento es un objeto que contiene en primer lugar y los apellidos.</span><span class="sxs-lookup"><span data-stu-id="ef93a-192">The `ng-init` directive establishes a `names` array, where each element is an object containing first and last names.</span></span> <span data-ttu-id="ef93a-193">El `ng-repeat` asignación, `name in names`, genera un elemento de lista para cada elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="ef93a-193">The `ng-repeat` assignment, `name in names`, outputs a list item for every array element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

<span data-ttu-id="ef93a-194">En este caso, el resultado es el mismo que en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="ef93a-194">The output in this case is the same as in the previous example.</span></span>

<span data-ttu-id="ef93a-195">Angular proporciona algunas directivas adicionales que pueden ayudar a proporcionar un comportamiento en función de donde está el bucle en su ejecución.</span><span class="sxs-lookup"><span data-stu-id="ef93a-195">Angular provides some additional directives that can help provide behavior based on where the loop is in its execution.</span></span>

`$index`

<span data-ttu-id="ef93a-196">Use `$index` en el `ng-repeat` bucle para determinar qué índice colocar el bucle actualmente se encuentra en.</span><span class="sxs-lookup"><span data-stu-id="ef93a-196">Use `$index` in the `ng-repeat` loop to determine which index position your loop currently is on.</span></span>

<span data-ttu-id="ef93a-197">`$even` y `$odd`</span><span class="sxs-lookup"><span data-stu-id="ef93a-197">`$even` and `$odd`</span></span>

<span data-ttu-id="ef93a-198">Use `$even` en el `ng-repeat` bucle para determinar si el índice actual en el bucle es una fila incluso indizada.</span><span class="sxs-lookup"><span data-stu-id="ef93a-198">Use `$even` in the `ng-repeat` loop to determine whether the current index in your loop is an even indexed row.</span></span> <span data-ttu-id="ef93a-199">De igual forma, utilizar `$odd` para determinar si el índice actual es una fila indizada impar.</span><span class="sxs-lookup"><span data-stu-id="ef93a-199">Similarly, use `$odd` to determine if the current index is an odd indexed row.</span></span>

<span data-ttu-id="ef93a-200">`$first` y `$last`</span><span class="sxs-lookup"><span data-stu-id="ef93a-200">`$first` and `$last`</span></span>

<span data-ttu-id="ef93a-201">Use `$first` en el `ng-repeat` bucle para determinar si el índice actual en el bucle es la primera fila.</span><span class="sxs-lookup"><span data-stu-id="ef93a-201">Use `$first` in the `ng-repeat` loop to determine whether the current index in your loop is the first row.</span></span> <span data-ttu-id="ef93a-202">De igual forma, utilizar `$last` para determinar si el índice actual es la última fila.</span><span class="sxs-lookup"><span data-stu-id="ef93a-202">Similarly, use `$last` to determine if the current index is the last row.</span></span>

<span data-ttu-id="ef93a-203">A continuación se muestra un ejemplo que muestra `$index`, `$even`, `$odd`, `$first`, y `$last` en acción:</span><span class="sxs-lookup"><span data-stu-id="ef93a-203">Below is a sample that shows `$index`, `$even`, `$odd`, `$first`, and `$last` in action:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

<span data-ttu-id="ef93a-204">Este es el resultado:</span><span class="sxs-lookup"><span data-stu-id="ef93a-204">Here is the resulting output:</span></span>

![Ejemplo de repetidor 2](angular/_static/repeaters2.png)

### <a name="scope"></a><span data-ttu-id="ef93a-206">$scope</span><span class="sxs-lookup"><span data-stu-id="ef93a-206">$scope</span></span>

<span data-ttu-id="ef93a-207">`$scope`es un objeto de JavaScript que actúa como adherencia entre la vista (plantilla) y el controlador (que se explica más adelante).</span><span class="sxs-lookup"><span data-stu-id="ef93a-207">`$scope` is a JavaScript object that acts as glue between the view (template) and the controller (explained below).</span></span> <span data-ttu-id="ef93a-208">Una plantilla de vista en AngularJS sólo sabe acerca de los valores que se adjunta a la `$scope` objeto en el controlador.</span><span class="sxs-lookup"><span data-stu-id="ef93a-208">A view template in AngularJS only knows about the values attached to the `$scope` object in the controller.</span></span>

> [!NOTE]
> <span data-ttu-id="ef93a-209">En el mundo MVVM, la `$scope` objeto en AngularJS a menudo se define como el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="ef93a-209">In the MVVM world, the `$scope` object in AngularJS is often defined as the ViewModel.</span></span> <span data-ttu-id="ef93a-210">El equipo de AngularJS hace referencia a la `$scope` los objetos según el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="ef93a-210">The AngularJS team refers to the `$scope` object as the Data-Model.</span></span> <span data-ttu-id="ef93a-211">[Más información sobre los ámbitos en AngularJS](https://docs.angularjs.org/guide/scope).</span><span class="sxs-lookup"><span data-stu-id="ef93a-211">[Learn more about Scopes in AngularJS](https://docs.angularjs.org/guide/scope).</span></span>

<span data-ttu-id="ef93a-212">A continuación se muestra un ejemplo sencillo que muestra cómo establecer las propiedades de `$scope` dentro de un archivo JavaScript independiente, *scope.js*:</span><span class="sxs-lookup"><span data-stu-id="ef93a-212">Below is a simple example showing how to set properties on `$scope` within a separate JavaScript file, *scope.js*:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

<span data-ttu-id="ef93a-213">Observe el `$scope` parámetro se pasa al controlador en la línea 2.</span><span class="sxs-lookup"><span data-stu-id="ef93a-213">Observe the `$scope` parameter passed to the controller on line 2.</span></span> <span data-ttu-id="ef93a-214">Este objeto es lo que conoce la vista.</span><span class="sxs-lookup"><span data-stu-id="ef93a-214">This object is what the view knows about.</span></span> <span data-ttu-id="ef93a-215">En la línea 3, estamos estableciendo una propiedad denominada "name" a "Mary Jane".</span><span class="sxs-lookup"><span data-stu-id="ef93a-215">On line 3, we are setting a property called "name" to "Mary Jane".</span></span>

<span data-ttu-id="ef93a-216">¿Qué ocurre cuando una propiedad determinada no se encuentra la vista?</span><span class="sxs-lookup"><span data-stu-id="ef93a-216">What happens when a particular property is not found by the view?</span></span> <span data-ttu-id="ef93a-217">La vista que se definen a continuación hace referencia a propiedades "name" y "age":</span><span class="sxs-lookup"><span data-stu-id="ef93a-217">The view defined below refers to "name" and "age" properties:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

<span data-ttu-id="ef93a-218">Tenga en cuenta en línea 9 que te pedimos Angular para mostrar la propiedad "name" con la sintaxis de expresión.</span><span class="sxs-lookup"><span data-stu-id="ef93a-218">Notice on line 9 that we are asking Angular to show the "name" property using expression syntax.</span></span> <span data-ttu-id="ef93a-219">Línea 10, a continuación, hace referencia a "age", una propiedad que no existe.</span><span class="sxs-lookup"><span data-stu-id="ef93a-219">Line 10 then refers to "age", a property that does not exist.</span></span> <span data-ttu-id="ef93a-220">Este ejemplo muestra el nombre establecido en "Mary Jane" y nada para una duración.</span><span class="sxs-lookup"><span data-stu-id="ef93a-220">The running example shows the name set to "Mary Jane" and nothing for age.</span></span> <span data-ttu-id="ef93a-221">Se omiten las propiedades que faltan.</span><span class="sxs-lookup"><span data-stu-id="ef93a-221">Missing properties are ignored.</span></span>

![Ejemplo de ámbito](angular/_static/scope.png)

### <a name="modules"></a><span data-ttu-id="ef93a-223">Módulos</span><span class="sxs-lookup"><span data-stu-id="ef93a-223">Modules</span></span>

<span data-ttu-id="ef93a-224">A [módulo](https://docs.angularjs.org/guide/module) en AngularJS es una colección de controladores, servicios, directivas, etcetera. El `angular.module()` llamada de función se utiliza para crear, registrar y recuperar los módulos de AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-224">A [module](https://docs.angularjs.org/guide/module) in AngularJS is a collection of controllers, services, directives, etc. The `angular.module()` function call is used to create, register, and retrieve modules in AngularJS.</span></span> <span data-ttu-id="ef93a-225">Todos los módulos, los enviados por el equipo de AngularJS y bibliotecas de terceros, incluidos deben registrarse mediante el `angular.module()` función.</span><span class="sxs-lookup"><span data-stu-id="ef93a-225">All modules, including those shipped by the AngularJS team and third party libraries, should be registered using the `angular.module()` function.</span></span>

<span data-ttu-id="ef93a-226">A continuación se muestra un fragmento de código que muestra cómo crear un nuevo módulo en AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-226">Below is a snippet of code that shows how to create a new module in AngularJS.</span></span> <span data-ttu-id="ef93a-227">El primer parámetro es el nombre del módulo.</span><span class="sxs-lookup"><span data-stu-id="ef93a-227">The first parameter is the name of the module.</span></span> <span data-ttu-id="ef93a-228">El segundo parámetro define las dependencias en otros módulos.</span><span class="sxs-lookup"><span data-stu-id="ef93a-228">The second parameter defines dependencies on other modules.</span></span> <span data-ttu-id="ef93a-229">Más adelante en este artículo, mostraremos cómo pasar estas dependencias para una `angular.module()` llamada al método.</span><span class="sxs-lookup"><span data-stu-id="ef93a-229">Later in this article, we will be showing how to pass these dependencies to an `angular.module()` method call.</span></span>

```javascript
var personApp = angular.module('personApp', []);
```

<span data-ttu-id="ef93a-230">Use la `ng-app` directiva para representar un módulo de AngularJS en la página.</span><span class="sxs-lookup"><span data-stu-id="ef93a-230">Use the `ng-app` directive to represent an AngularJS module on the page.</span></span> <span data-ttu-id="ef93a-231">Para utilizar un módulo, asigne el nombre del módulo, `personApp` en este ejemplo, para el `ng-app` la directiva en la plantilla.</span><span class="sxs-lookup"><span data-stu-id="ef93a-231">To use a module, assign the name of the module, `personApp` in this example, to the `ng-app` directive in our template.</span></span>

```html
<body ng-app="personApp">
```

### <a name="controllers"></a><span data-ttu-id="ef93a-232">Controladores</span><span class="sxs-lookup"><span data-stu-id="ef93a-232">Controllers</span></span>

<span data-ttu-id="ef93a-233">[Controladores de](https://docs.angularjs.org/guide/controller) en AngularJS son el primer punto de entrada para el código.</span><span class="sxs-lookup"><span data-stu-id="ef93a-233">[Controllers](https://docs.angularjs.org/guide/controller) in AngularJS are the first point of entry for your code.</span></span> <span data-ttu-id="ef93a-234">El `<module name>.controller()` llamada de función se utiliza para crear y registrar los controladores de AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-234">The `<module name>.controller()` function call is used to create and register controllers in AngularJS.</span></span> <span data-ttu-id="ef93a-235">El `ng-controller` directiva se usa para representar un controlador de AngularJS en la página HTML.</span><span class="sxs-lookup"><span data-stu-id="ef93a-235">The `ng-controller` directive is used to represent an AngularJS controller on the HTML page.</span></span> <span data-ttu-id="ef93a-236">El rol del controlador en Angular consiste en establecer el estado y el comportamiento del modelo de datos (`$scope`).</span><span class="sxs-lookup"><span data-stu-id="ef93a-236">The role of the controller in Angular is to set state and behavior of the data model (`$scope`).</span></span> <span data-ttu-id="ef93a-237">Controladores no deben utilizarse para manipular el DOM directamente.</span><span class="sxs-lookup"><span data-stu-id="ef93a-237">Controllers should not be used to manipulate the DOM directly.</span></span>

<span data-ttu-id="ef93a-238">A continuación se muestra un fragmento de código que se registra un nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="ef93a-238">Below is a snippet of code that registers a new controller.</span></span> <span data-ttu-id="ef93a-239">El `personApp` variable en el fragmento de código hace referencia a un módulo Angular, que se define en la línea 2.</span><span class="sxs-lookup"><span data-stu-id="ef93a-239">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

<span data-ttu-id="ef93a-240">La vista que utiliza el `ng-controller` directiva asigna el nombre del controlador:</span><span class="sxs-lookup"><span data-stu-id="ef93a-240">The view using the `ng-controller` directive assigns the controller name:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

<span data-ttu-id="ef93a-241">La página muestra "Mary" y "Jane" que corresponden a la `firstName` y `lastName` propiedades adjunto a la `$scope` objeto:</span><span class="sxs-lookup"><span data-stu-id="ef93a-241">The page shows "Mary" and "Jane" that correspond to the `firstName` and `lastName` properties attached to the `$scope` object:</span></span>

![Ejemplo de controlador](angular/_static/controllers.png)

### <a name="components"></a><span data-ttu-id="ef93a-243">Componentes</span><span class="sxs-lookup"><span data-stu-id="ef93a-243">Components</span></span>

<span data-ttu-id="ef93a-244">[Componentes](https://docs.angularjs.org/guide/component) en Angular 1.5. x permiten la encapsulación y la capacidad de crear los elementos individuales de HTML.</span><span class="sxs-lookup"><span data-stu-id="ef93a-244">[Components](https://docs.angularjs.org/guide/component) in Angular 1.5.x allow for the encapsulation and capability of creating individual HTML elements.</span></span> <span data-ttu-id="ef93a-245">En Angular 1.4. x podría conseguir la misma función mediante el método .directive().</span><span class="sxs-lookup"><span data-stu-id="ef93a-245">In Angular 1.4.x you could achieve the same feature using the .directive() method.</span></span>

<span data-ttu-id="ef93a-246">Mediante el método de .component(), desarrollo se simplifica obtengan la funcionalidad de la directiva y el controlador.</span><span class="sxs-lookup"><span data-stu-id="ef93a-246">By using the .component() method, development is simplified gaining the functionality of the directive and the controller.</span></span> <span data-ttu-id="ef93a-247">Otras ventajas incluyen; aislamiento de ámbito, los procedimientos recomendados son inherentes y migración a 2 Angular se convierte en una tarea más fácil.</span><span class="sxs-lookup"><span data-stu-id="ef93a-247">Other benefits include; scope isolation, best practices are inherent, and migration to Angular 2 becomes an easier task.</span></span> <span data-ttu-id="ef93a-248">El `<module name>.component()` llamada de función se utiliza para crear y registrar los componentes de AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-248">The `<module name>.component()` function call is used to create and register components in AngularJS.</span></span>

<span data-ttu-id="ef93a-249">A continuación se muestra un fragmento de código que se registra un nuevo componente.</span><span class="sxs-lookup"><span data-stu-id="ef93a-249">Below is a snippet of code that registers a new component.</span></span> <span data-ttu-id="ef93a-250">El `personApp` variable en el fragmento de código hace referencia a un módulo Angular, que se define en la línea 2.</span><span class="sxs-lookup"><span data-stu-id="ef93a-250">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

<span data-ttu-id="ef93a-251">La vista donde estamos mostrando el elemento HTML personalizado.</span><span class="sxs-lookup"><span data-stu-id="ef93a-251">The view where we are displaying the custom HTML element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

<span data-ttu-id="ef93a-252">La plantilla asociada componente utilizada:</span><span class="sxs-lookup"><span data-stu-id="ef93a-252">The associated template used by component:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

<span data-ttu-id="ef93a-253">La página muestra "Aftab" y "Ansari" que corresponden a la `firstName` y `lastName` propiedades adjunto a la `vm` objeto:</span><span class="sxs-lookup"><span data-stu-id="ef93a-253">The page shows "Aftab" and "Ansari" that correspond to the `firstName` and `lastName` properties attached to the `vm` object:</span></span>

![Ejemplo de componentes](angular/_static/components.png)

### <a name="services"></a><span data-ttu-id="ef93a-255">Servicios</span><span class="sxs-lookup"><span data-stu-id="ef93a-255">Services</span></span>

<span data-ttu-id="ef93a-256">[Servicios](https://docs.angularjs.org/guide/services) en AngularJS se usan normalmente para código compartido que se resume en un archivo que se puede usar durante el ciclo de vida de una aplicación Angular.</span><span class="sxs-lookup"><span data-stu-id="ef93a-256">[Services](https://docs.angularjs.org/guide/services) in AngularJS are commonly used for shared code that is abstracted away into a file which can be used throughout the lifetime of an Angular application.</span></span> <span data-ttu-id="ef93a-257">Servicios haya instancias de forma diferida, lo que significa que no habrá una instancia de un servicio a menos que se usa un componente que depende del servicio.</span><span class="sxs-lookup"><span data-stu-id="ef93a-257">Services are lazily instantiated, meaning that there will not be an instance of a service unless a component that depends on the service gets used.</span></span> <span data-ttu-id="ef93a-258">Los generadores son un ejemplo de un servicio utilizado en las aplicaciones de AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-258">Factories are an example of a service used in AngularJS applications.</span></span> <span data-ttu-id="ef93a-259">Generadores de se crean mediante el `myApp.factory()` función llamada, donde `myApp` es el módulo.</span><span class="sxs-lookup"><span data-stu-id="ef93a-259">Factories are created using the `myApp.factory()` function call, where `myApp` is the module.</span></span>

<span data-ttu-id="ef93a-260">A continuación se muestra un ejemplo que muestra cómo utilizar generadores en AngularJS:</span><span class="sxs-lookup"><span data-stu-id="ef93a-260">Below is an example that shows how to use factories in AngularJS:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

<span data-ttu-id="ef93a-261">Para llamar a este generador del controlador, pasar `personFactory` como un parámetro a la `controller` función:</span><span class="sxs-lookup"><span data-stu-id="ef93a-261">To call this factory from the controller, pass `personFactory` as a parameter to the `controller` function:</span></span>

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a><span data-ttu-id="ef93a-262">Utilizar los servicios para comunicarse con un punto de conexión REST</span><span class="sxs-lookup"><span data-stu-id="ef93a-262">Using services to talk to a REST endpoint</span></span>

<span data-ttu-id="ef93a-263">A continuación se muestra un ejemplo de extremo a extremo usando servicios en AngularJS para interactuar con un punto de conexión de API Web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef93a-263">Below is an end-to-end example using services in AngularJS to interact with an ASP.NET Core Web API endpoint.</span></span> <span data-ttu-id="ef93a-264">El ejemplo obtiene los datos de la API Web y muestra los datos en una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="ef93a-264">The example gets data from the Web API and displays the data in a view template.</span></span> <span data-ttu-id="ef93a-265">Comenzaremos con la vista en primer lugar:</span><span class="sxs-lookup"><span data-stu-id="ef93a-265">Let's start with the view first:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

<span data-ttu-id="ef93a-266">En esta vista, tenemos un módulo Angular denominado `PersonsApp` y llama a un controlador `personController`.</span><span class="sxs-lookup"><span data-stu-id="ef93a-266">In this view, we have an Angular module called `PersonsApp` and a controller called `personController`.</span></span> <span data-ttu-id="ef93a-267">Estamos usando `ng-repeat` para recorrer en iteración la lista de personas.</span><span class="sxs-lookup"><span data-stu-id="ef93a-267">We are using `ng-repeat` to iterate over the list of persons.</span></span> <span data-ttu-id="ef93a-268">Hacemos referencia a tres archivos JavaScript personalizados en líneas 17-19.</span><span class="sxs-lookup"><span data-stu-id="ef93a-268">We are referencing three custom JavaScript files on lines 17-19.</span></span>

<span data-ttu-id="ef93a-269">El *personApp.js* archivo se usa para registrar el `PersonsApp` módulo; y, la sintaxis es similar a los ejemplos anteriores.</span><span class="sxs-lookup"><span data-stu-id="ef93a-269">The *personApp.js* file is used to register the `PersonsApp` module; and, the syntax is similar to previous examples.</span></span> <span data-ttu-id="ef93a-270">Usamos el `angular.module` función para crear una nueva instancia del módulo que se va a trabajar.</span><span class="sxs-lookup"><span data-stu-id="ef93a-270">We are using the `angular.module` function to create a new instance of the module that we will be working with.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

<span data-ttu-id="ef93a-271">¡Eche un vistazo a *personFactory.js*, más adelante.</span><span class="sxs-lookup"><span data-stu-id="ef93a-271">Let's take a look at *personFactory.js*, below.</span></span> <span data-ttu-id="ef93a-272">Estamos llamando a del módulo `factory` método para crear un generador.</span><span class="sxs-lookup"><span data-stu-id="ef93a-272">We are calling the module’s `factory` method to create a factory.</span></span> <span data-ttu-id="ef93a-273">La línea 12 muestra el Angular integrado `$http` servicio de recuperación de información de usuarios de un servicio web.</span><span class="sxs-lookup"><span data-stu-id="ef93a-273">Line 12 shows the built-in Angular `$http` service retrieving people information from a web service.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

<span data-ttu-id="ef93a-274">En *personController.js*, vamos a llamar el módulo `controller` método para crear el controlador.</span><span class="sxs-lookup"><span data-stu-id="ef93a-274">In *personController.js*, we are calling the module’s `controller` method to create the controller.</span></span> <span data-ttu-id="ef93a-275">El `$scope` del objeto `people` propiedad se asigna a los datos devueltos por la personFactory (línea 13).</span><span class="sxs-lookup"><span data-stu-id="ef93a-275">The `$scope` object's `people` property is assigned the data returned from the personFactory (line 13).</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

<span data-ttu-id="ef93a-276">¡Eche un vistazo a la API de Web y el modelo subyacente.</span><span class="sxs-lookup"><span data-stu-id="ef93a-276">Let's take a quick look at the Web API and the model behind it.</span></span> <span data-ttu-id="ef93a-277">El `Person` modelo es un POCO (objeto CLR antiguos sin formato) con `Id`, `FirstName`, y `LastName` propiedades:</span><span class="sxs-lookup"><span data-stu-id="ef93a-277">The `Person` model is a POCO (Plain Old CLR Object) with `Id`, `FirstName`, and `LastName` properties:</span></span>

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

<span data-ttu-id="ef93a-278">El `Person` controlador devuelve una lista con formato JSON de `Person` objetos:</span><span class="sxs-lookup"><span data-stu-id="ef93a-278">The `Person` controller returns a JSON-formatted list of `Person` objects:</span></span>

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

<span data-ttu-id="ef93a-279">Vamos a ver la aplicación en acción:</span><span class="sxs-lookup"><span data-stu-id="ef93a-279">Let's see the application in action:</span></span>

![Resultado del controlador de mostrar REST](angular/_static/rest-bound.png)

<span data-ttu-id="ef93a-281">También puede [ver la estructura de aplicación en GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="ef93a-281">You can [view the application's structure on GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="ef93a-282">Para obtener más información sobre la estructura de las aplicaciones de AngularJS, consulte [Angular Guía de John Papa de estilo](https://github.com/johnpapa/angular-styleguide)</span><span class="sxs-lookup"><span data-stu-id="ef93a-282">For more on structuring AngularJS applications, see [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide)</span></span>

&nbsp;

> [!NOTE]
> <span data-ttu-id="ef93a-283">Para crear el módulo de AngularJS, controlador, generador, archivos de directiva y vista fácilmente, asegúrese de que desproteja Sayed Hashimi [SideWaffle el paquete de plantillas de Visual Studio](http://sidewaffle.com/).</span><span class="sxs-lookup"><span data-stu-id="ef93a-283">To create AngularJS module, controller, factory, directive and view files easily, be sure to check out Sayed Hashimi's [SideWaffle template pack for Visual Studio](http://sidewaffle.com/).</span></span> <span data-ttu-id="ef93a-284">Sayed Hashimi es un director de programas en el equipo de Web de Visual Studio en Microsoft y plantillas de SideWaffle se consideran el estándar de oro.</span><span class="sxs-lookup"><span data-stu-id="ef93a-284">Sayed Hashimi is a Senior Program Manager on the Visual Studio Web Team at Microsoft and SideWaffle templates are considered the gold standard.</span></span> <span data-ttu-id="ef93a-285">En el momento de redactar este artículo, SideWaffle está disponible para Visual Studio 2012, 2013 y 2015.</span><span class="sxs-lookup"><span data-stu-id="ef93a-285">At the time of this writing, SideWaffle is available for Visual Studio 2012, 2013, and 2015.</span></span>

### <a name="routing-and-multiple-views"></a><span data-ttu-id="ef93a-286">Enrutamiento y varias vistas</span><span class="sxs-lookup"><span data-stu-id="ef93a-286">Routing and multiple views</span></span>

<span data-ttu-id="ef93a-287">AngularJS tiene un proveedor de ruta integrados para controlar la navegación de SPA (aplicación de página única) en función.</span><span class="sxs-lookup"><span data-stu-id="ef93a-287">AngularJS has a built-in route provider to handle SPA (Single Page Application) based navigation.</span></span> <span data-ttu-id="ef93a-288">Para trabajar con el enrutamiento de AngularJS, debe agregar el `angular-route` biblioteca mediante Bower.</span><span class="sxs-lookup"><span data-stu-id="ef93a-288">To work with routing in AngularJS, you must add the `angular-route` library using Bower.</span></span> <span data-ttu-id="ef93a-289">Puede ver en la [bower.json](#angular-bower-json) archivo al que hace referencia al principio de este artículo que se estamos ya hace referencia a él en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ef93a-289">You can see in the [bower.json](#angular-bower-json) file referenced at the start of this article that we are already referencing it in our project.</span></span>

<span data-ttu-id="ef93a-290">Después de instalar el paquete, agregue la referencia de script (*route.js angular*) a la vista.</span><span class="sxs-lookup"><span data-stu-id="ef93a-290">After you install the package, add the script reference (*angular-route.js*) to your view.</span></span>

<span data-ttu-id="ef93a-291">Ahora veamos la aplicación de la persona que se han ido acumulando y agregar funciones de navegación a él.</span><span class="sxs-lookup"><span data-stu-id="ef93a-291">Now let's take the Person App we have been building and add navigation to it.</span></span> <span data-ttu-id="ef93a-292">En primer lugar, se realizará una copia de la aplicación creando un nuevo `PeopleController` acción denominada `Spa` y su correspondiente `Spa.cshtml` vista mediante la copia de la vista de Index.cshtml en el `People` carpeta.</span><span class="sxs-lookup"><span data-stu-id="ef93a-292">First, we will make a copy of the app by creating a new `PeopleController` action called `Spa` and a corresponding `Spa.cshtml` view by copying the Index.cshtml view in the `People` folder.</span></span> <span data-ttu-id="ef93a-293">Agregue una referencia de script a `angular-route` (vea la línea 11).</span><span class="sxs-lookup"><span data-stu-id="ef93a-293">Add a script reference to `angular-route` (see line 11).</span></span> <span data-ttu-id="ef93a-294">Agregar un `div` marcados con el `ng-view` directiva (vea la línea 6) como marcador de posición para colocar las vistas en.</span><span class="sxs-lookup"><span data-stu-id="ef93a-294">Also add a `div` marked with the `ng-view` directive (see line 6) as a placeholder to place views in.</span></span> <span data-ttu-id="ef93a-295">Vamos a usar varias adicionales *.js* archivos que se hace referencia en las líneas 13-16.</span><span class="sxs-lookup"><span data-stu-id="ef93a-295">We are going to be using several additional *.js* files which are referenced on lines 13-16.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

<span data-ttu-id="ef93a-296">¡Eche un vistazo a *personModule.js* archivo para ver cómo se estamos crear instancias del módulo con el enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="ef93a-296">Let's take a look at *personModule.js* file to see how we are instantiating the module with routing.</span></span> <span data-ttu-id="ef93a-297">Pasamos `ngRoute` como una biblioteca en el módulo.</span><span class="sxs-lookup"><span data-stu-id="ef93a-297">We are passing `ngRoute` as a library into the module.</span></span> <span data-ttu-id="ef93a-298">Este módulo controla el enrutamiento en nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef93a-298">This module handles routing in our application.</span></span>

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

<span data-ttu-id="ef93a-299">El *personRoutes.js* archivo, a continuación, define las rutas basándose en el proveedor de ruta.</span><span class="sxs-lookup"><span data-stu-id="ef93a-299">The *personRoutes.js* file, below, defines routes based on the route provider.</span></span> <span data-ttu-id="ef93a-300">Las líneas 4-7 definen la navegación eficazmente diciendo, cuando una dirección URL con `/persons` es solicitado, utilizar una plantilla denominada `partials/personlist` por trabajar a través de `personListController`.</span><span class="sxs-lookup"><span data-stu-id="ef93a-300">Lines 4-7 define navigation by effectively saying, when a URL with `/persons` is requested, use a template called `partials/personlist` by working through `personListController`.</span></span> <span data-ttu-id="ef93a-301">Líneas del 8 al 11 indican una página de detalles con un parámetro de ruta de `personId`.</span><span class="sxs-lookup"><span data-stu-id="ef93a-301">Lines 8-11 indicate a detail page with a route parameter of `personId`.</span></span> <span data-ttu-id="ef93a-302">Si la dirección URL no coincide con uno de los modelos, Angular tiene como valor predeterminado el `/persons` vista.</span><span class="sxs-lookup"><span data-stu-id="ef93a-302">If the URL doesn't match one of the patterns, Angular defaults to the `/persons` view.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

<span data-ttu-id="ef93a-303">El `personlist.html` archivo es una vista parcial que contiene solo el código HTML necesario para mostrar la lista de personas.</span><span class="sxs-lookup"><span data-stu-id="ef93a-303">The `personlist.html` file is a partial view containing only the HTML needed to display person list.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

<span data-ttu-id="ef93a-304">El controlador se define por medio del módulo `controller` funcionando en *personListController.js*.</span><span class="sxs-lookup"><span data-stu-id="ef93a-304">The controller is defined by using the module's `controller` function in *personListController.js*.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

<span data-ttu-id="ef93a-305">Si se ejecuta esta aplicación y navegue hasta el `people/spa#/persons` dirección URL, veremos:</span><span class="sxs-lookup"><span data-stu-id="ef93a-305">If we run this application and navigate to the `people/spa#/persons` URL, we will see:</span></span>

![Vista de lista de personas](angular/_static/spa-persons.png)

<span data-ttu-id="ef93a-307">Si se vaya a una página de detalles, como por ejemplo `people/spa#/persons/2`, vemos la vista parcial de detalle:</span><span class="sxs-lookup"><span data-stu-id="ef93a-307">If we navigate to a detail page, for example `people/spa#/persons/2`, we will see the detail partial view:</span></span>

![Vista de detalle de persona](angular/_static/spa-persons-2.png)

<span data-ttu-id="ef93a-309">Puede ver todo el código fuente y los archivos no se muestran en este artículo en [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="ef93a-309">You can view the full source and any files not shown in this article on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

### <a name="event-handlers"></a><span data-ttu-id="ef93a-310">Controladores de eventos</span><span class="sxs-lookup"><span data-stu-id="ef93a-310">Event Handlers</span></span>

<span data-ttu-id="ef93a-311">Hay una serie de directivas de AngularJS que agregar capacidades de control de eventos a los elementos de entrada en el HTML DOM.</span><span class="sxs-lookup"><span data-stu-id="ef93a-311">There are a number of directives in AngularJS that add event-handling capabilities to the input elements in your HTML DOM.</span></span> <span data-ttu-id="ef93a-312">A continuación se muestra una lista de los eventos que se integran en AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-312">Below is a list of the events that are built into AngularJS.</span></span>

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> <span data-ttu-id="ef93a-313">Puede agregar sus propios controladores de eventos mediante el [directivas personalizadas de características en AngularJS](https://docs.angularjs.org/guide/directive).</span><span class="sxs-lookup"><span data-stu-id="ef93a-313">You can add your own event handlers using the [custom directives feature in AngularJS](https://docs.angularjs.org/guide/directive).</span></span>

<span data-ttu-id="ef93a-314">Echemos un vistazo a cómo el `ng-click` está dispuesto eventos.</span><span class="sxs-lookup"><span data-stu-id="ef93a-314">Let's look at how the `ng-click` event is wired up.</span></span> <span data-ttu-id="ef93a-315">Cree un nuevo archivo JavaScript denominado *eventHandlerController.js*y agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ef93a-315">Create a new JavaScript file named *eventHandlerController.js*, and add the following to it:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

<span data-ttu-id="ef93a-316">Tenga en cuenta la nueva `sayName` funcionando en `eventHandlerController` en línea 5 anterior.</span><span class="sxs-lookup"><span data-stu-id="ef93a-316">Notice the new `sayName` function in `eventHandlerController` on line 5 above.</span></span> <span data-ttu-id="ef93a-317">Realiza todo el método para ahora se muestra una alerta de JavaScript al usuario con un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="ef93a-317">All the method is doing for now is showing a JavaScript alert to the user with a welcome message.</span></span>

<span data-ttu-id="ef93a-318">La vista siguiente enlaza una función de controlador a un evento de AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-318">The view below binds a controller function to an AngularJS event.</span></span> <span data-ttu-id="ef93a-319">La línea 9 tiene un botón en el que el `ng-click` se ha aplicado la directiva Angular.</span><span class="sxs-lookup"><span data-stu-id="ef93a-319">Line 9 has a button on which the `ng-click` Angular directive has been applied.</span></span> <span data-ttu-id="ef93a-320">Llama a nuestro `sayName` función, que está conectado a la `$scope` objeto pasa a esta vista.</span><span class="sxs-lookup"><span data-stu-id="ef93a-320">It calls our `sayName` function, which is attached to the `$scope` object passed to this view.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

<span data-ttu-id="ef93a-321">Este ejemplo muestra que el controlador `sayName` función se invoca automáticamente cuando se hace clic en el botón.</span><span class="sxs-lookup"><span data-stu-id="ef93a-321">The running example demonstrates that the controller's `sayName` function is called automatically when the button is clicked.</span></span>

![Click (evento)](angular/_static/events.png)

<span data-ttu-id="ef93a-323">Para obtener más detalles sobre directivas de controlador de eventos integrados de AngularJS, asegúrese de encabezado hasta el [sitio Web de documentación](https://docs.angularjs.org/api/ng/directive/ngClick) de AngularJS.</span><span class="sxs-lookup"><span data-stu-id="ef93a-323">For more detail on AngularJS built-in event handler directives, be sure to head to the [documentation website](https://docs.angularjs.org/api/ng/directive/ngClick) of AngularJS.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef93a-324">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ef93a-324">Additional resources</span></span>

* [<span data-ttu-id="ef93a-325">Documentos angulares</span><span class="sxs-lookup"><span data-stu-id="ef93a-325">Angular Docs</span></span>](https://docs.angularjs.org)

* [<span data-ttu-id="ef93a-326">Información de 2 angular</span><span class="sxs-lookup"><span data-stu-id="ef93a-326">Angular 2 Info</span></span>](https://angular.io/)
