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
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>Uso de AngularJS para aplicaciones de una página (SPAs) con ASP.NET Core


Por [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) y [Scott Addie](https://scottaddie.com)

En este artículo, aprenderá cómo crear una aplicación de ASP.NET SPA estilo con AngularJS.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-angularjs"></a>¿Qué es AngularJS?

[AngularJS](https://angularjs.org/) es un marco de JavaScript moderno de Google que se usa normalmente para trabajar con aplicaciones de una sola página (SPAs). AngularJS está abierto con origen bajo licencia MIT y puede seguir el progreso de la programación de AngularJS [su repositorio de GitHub](https://github.com/angular/angular.js). La biblioteca se denomina Angular porque HTML utiliza corchetes angulares en forma.

AngularJS no es una biblioteca de manipulación de DOM como jQuery, pero utiliza un subconjunto de jQuery denominado jQLite. AngularJS se basa principalmente en los atributos declarativos de HTML que se pueden agregar a las etiquetas HTML. Puede intentar AngularJS en el explorador mediante la [sitio Web de código School](https://www.codeschool.com/courses/shaping-up-with-angularjs) o [sitio Web de W3Schools](https://www.w3schools.com/angular/).

En este artículo se centra en AngularJS con algunas notas en donde se dirige Angular.

## <a name="getting-started"></a>Introducción

Para empezar a usar AngularJS en la aplicación ASP.NET, debe instalar como parte de su proyecto o hacer referencia a él desde una red de entrega de contenido (CDN).

### <a name="installation"></a>Instalación

Hay varias maneras de agregar AngularJS a la aplicación. Si está iniciando una nueva aplicación web de ASP.NET Core en Visual Studio, puede agregar AngularJS mediante integrado [Bower](bower.md) admite. Abra *bower.json*y agregar una entrada a la `dependencies` propiedad:

<a name=angular-bower-json></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

Al guardar la *bower.json* archivo, Angular se instalará en el proyecto *wwwroot/lib* carpeta. Además, se mostrarán en el `Dependencies/Bower` carpeta. Consulte la captura de pantalla siguiente.

![Explorador de soluciones con proyectos de AngularJS](angular/_static/angular-solution-explorer.png)

A continuación, agregue un `<script>` referencia a la parte inferior de la `<body>` sección de la página HTML o *_Layout.cshtml* de archivos, como se muestra aquí:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

Se recomienda que las aplicaciones de producción usan CDN para las bibliotecas comunes como AngularJS. Puede hacer referencia a AngularJS desde uno de varios CDN, como éste:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

Una vez que tenga una referencia a la *angular.js* archivo de script, está listo para comenzar a usar AngularJS en las páginas web.

## <a name="key-components"></a>Componentes clave

AngularJS incluye una serie de componentes principales, como *directivas*, *plantillas*, *repetidores*, *módulos*,  *controladores de*, *componentes*, *enrutador componentes* y mucho más. Vamos a examinar cómo funcionan juntos estos componentes para agregar un comportamiento a las páginas web.

### <a name="directives"></a>Directivas

Usa AngularJS [directivas](https://docs.angularjs.org/guide/directive) extender HTML con elementos y atributos personalizados. Directivas de AngularJS se definen a través de `data-ng-*` o `ng-*` prefijos (`ng` es una abreviación de angular). Hay dos tipos de directivas de AngularJS:

   1. **Directivas primitivas**: estos predefinidos por el equipo Angular y forman parte del marco AngularJS.

   2. **Las directivas personalizadas de**: se trata de las directivas personalizadas que se pueden definir.

Una de las directivas de primitivas utilizadas en todas las aplicaciones de AngularJS es la `ng-app` directiva, que ejecuta la aplicación de AngularJS un bootstrap. Esta directiva puede aplicarse a la `<body>` etiqueta o a un elemento secundario del cuerpo. Veamos un ejemplo en acción. Suponiendo que esté en un proyecto de ASP.NET, puede agregar un archivo HTML para la `wwwroot` carpeta, o agregar una nueva acción de controlador y una vista asociada. En este caso, he agregado un nuevo `Directives` método de acción para `HomeController.cs`. La vista asociada se muestra aquí:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

Para mantener estos ejemplos independientes entre sí, no estoy usando el archivo de diseño compartido. Puede ver que es representativo de la etiqueta body con el `ng-app` directiva para indicar esta página es una aplicación de AngularJS. El `{{2+2}}` es una expresión de enlace de datos Angular aprenderá más acerca de en unos instantes. Este es el resultado si se ejecuta esta aplicación:

![Directiva Angular simple](angular/_static/simple-directive.png)

Incluyen otras directivas primitivos en AngularJS:

`ng-controller`Determina qué controlador de JavaScript está enlazado a la vista.

`ng-model`Determina el modelo al que se enlazan los valores de propiedades de un elemento HTML.

`ng-init`Se utiliza para inicializar los datos de aplicación en forma de una expresión para el ámbito actual.

`ng-if`Quita o vuelve a crear el elemento HTML especificado en el DOM tomando como base el truthiness de la expresión proporcionada.

`ng-repeat`Repite un bloque de HTML determinado a través de un conjunto de datos.

`ng-show`Muestra u oculta el elemento HTML especificado en función de la expresión proporcionada.

Para obtener una lista completa de todas las directivas de primitivas admitidos en AngularJS, consulte el [sección de documentación de la directiva en el sitio Web de documentación de AngularJS](https://docs.angularjs.org/api/ng/directive).

### <a name="data-binding"></a>Enlace de datos

Proporciona AngularJS [enlace de datos](https://docs.angularjs.org/guide/databinding) admite out-of-the-box mediante la `ng-bind` directiva o una sintaxis de expresión de enlace como de datos `{{expression}}`. AngularJS admite el enlace de datos bidireccional que se conservan los datos de un modelo en la sincronización con una plantilla de vista en todo momento. Los cambios realizados en la vista se reflejan automáticamente en el modelo. Del mismo modo, los cambios en el modelo se reflejan en la vista.

Crear un archivo HTML o una acción de controlador con una vista que lo acompaña denominada `Databinding`. Incluir lo siguiente en la vista:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

Observe que puede mostrar los valores del modelo mediante el enlace de directivas o datos (`ng-bind`). La página resultante debería tener este aspecto:

![Enlace de datos simple](angular/_static/simple-databinding.png)

### <a name="templates"></a>Plantillas

[Plantillas de](https://docs.angularjs.org/guide/templates) en AngularJS páginas HTML sin formato decoradas con directivas de AngularJS y artefactos. Una plantilla de AngularJS es una combinación de directivas, expresiones, filtros y controles que se combinan con HTML para la vista de formulario.

Agregar otra vista para mostrar las plantillas y agregue lo siguiente:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

La plantilla tiene directivas de AngularJS como `ng-app`, `ng-init`, `ng-model` y sintaxis de expresiones de enlace de datos para enlazar el `personName` propiedad. Se ejecuta en el explorador, la vista es similar a la captura de pantalla siguiente:

![Ejemplo de plantillas simple 1](angular/_static/simple-templates-1.png)

Si cambia el nombre, escriba en el campo de entrada, se mostrará el texto situado junto al campo de entrada dinámicamente update, que muestra el enlace de datos bidireccional Angular en acción.

![Ejemplo de plantillas simple 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>Expresiones

[Expresiones](https://docs.angularjs.org/guide/expression) en AngularJS son fragmentos de código similar a JavaScript que se escriben en el `{{ expression }}` sintaxis. Los datos de estas expresiones se enlazan a HTML del mismo modo que `ng-bind` directivas. La diferencia principal entre las expresiones regulares de JavaScript y expresiones de AngularJS es ese AngularJS expresiones se evalúan con la `$scope` objeto en AngularJS.

Las expresiones de AngularJS en el ejemplo a continuación enlace `personName` y JavaScript simple calcula la expresión:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

En el ejemplo se ejecuta en el explorador muestra el `personName` datos y los resultados del cálculo:

![Expresiones simples](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>Repetidores

Repetir en AngularJS se realiza a través de una directiva primitiva denominada `ng-repeat`. El `ng-repeat` directiva repite un determinado elemento HTML en una vista a lo largo de una matriz de datos repetidos. Pueden repetir repetidores en AngularJS a través de una matriz de cadenas u objetos. Este es un ejemplo de uso de repetición en una matriz de cadenas:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

El [repeat (directiva)](https://docs.angularjs.org/api/ng/directive/ngRepeat) genera una serie de elementos de lista en una lista sin ordenar, como puede ver en las herramientas de desarrollo que se muestra en esta captura de pantalla:

![Ejemplo de repetidor](angular/_static/repeater.png)

Este es un ejemplo que se repite en una matriz de objetos. El `ng-init` Directiva establece una `names` matriz, donde cada elemento es un objeto que contiene en primer lugar y los apellidos. El `ng-repeat` asignación, `name in names`, genera un elemento de lista para cada elemento de matriz.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

En este caso, el resultado es el mismo que en el ejemplo anterior.

Angular proporciona algunas directivas adicionales que pueden ayudar a proporcionar un comportamiento en función de donde está el bucle en su ejecución.

`$index`

Use `$index` en el `ng-repeat` bucle para determinar qué índice colocar el bucle actualmente se encuentra en.

`$even` y `$odd`

Use `$even` en el `ng-repeat` bucle para determinar si el índice actual en el bucle es una fila incluso indizada. De igual forma, utilizar `$odd` para determinar si el índice actual es una fila indizada impar.

`$first` y `$last`

Use `$first` en el `ng-repeat` bucle para determinar si el índice actual en el bucle es la primera fila. De igual forma, utilizar `$last` para determinar si el índice actual es la última fila.

A continuación se muestra un ejemplo que muestra `$index`, `$even`, `$odd`, `$first`, y `$last` en acción:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

Este es el resultado:

![Ejemplo de repetidor 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`es un objeto de JavaScript que actúa como adherencia entre la vista (plantilla) y el controlador (que se explica más adelante). Una plantilla de vista en AngularJS sólo sabe acerca de los valores que se adjunta a la `$scope` objeto en el controlador.

> [!NOTE]
> En el mundo MVVM, la `$scope` objeto en AngularJS a menudo se define como el modelo de vista. El equipo de AngularJS hace referencia a la `$scope` los objetos según el modelo de datos. [Más información sobre los ámbitos en AngularJS](https://docs.angularjs.org/guide/scope).

A continuación se muestra un ejemplo sencillo que muestra cómo establecer las propiedades de `$scope` dentro de un archivo JavaScript independiente, *scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

Observe el `$scope` parámetro se pasa al controlador en la línea 2. Este objeto es lo que conoce la vista. En la línea 3, estamos estableciendo una propiedad denominada "name" a "Mary Jane".

¿Qué ocurre cuando una propiedad determinada no se encuentra la vista? La vista que se definen a continuación hace referencia a propiedades "name" y "age":

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

Tenga en cuenta en línea 9 que te pedimos Angular para mostrar la propiedad "name" con la sintaxis de expresión. Línea 10, a continuación, hace referencia a "age", una propiedad que no existe. Este ejemplo muestra el nombre establecido en "Mary Jane" y nada para una duración. Se omiten las propiedades que faltan.

![Ejemplo de ámbito](angular/_static/scope.png)

### <a name="modules"></a>Módulos

A [módulo](https://docs.angularjs.org/guide/module) en AngularJS es una colección de controladores, servicios, directivas, etcetera. El `angular.module()` llamada de función se utiliza para crear, registrar y recuperar los módulos de AngularJS. Todos los módulos, los enviados por el equipo de AngularJS y bibliotecas de terceros, incluidos deben registrarse mediante el `angular.module()` función.

A continuación se muestra un fragmento de código que muestra cómo crear un nuevo módulo en AngularJS. El primer parámetro es el nombre del módulo. El segundo parámetro define las dependencias en otros módulos. Más adelante en este artículo, mostraremos cómo pasar estas dependencias para una `angular.module()` llamada al método.

```javascript
var personApp = angular.module('personApp', []);
```

Use la `ng-app` directiva para representar un módulo de AngularJS en la página. Para utilizar un módulo, asigne el nombre del módulo, `personApp` en este ejemplo, para el `ng-app` la directiva en la plantilla.

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>Controladores

[Controladores de](https://docs.angularjs.org/guide/controller) en AngularJS son el primer punto de entrada para el código. El `<module name>.controller()` llamada de función se utiliza para crear y registrar los controladores de AngularJS. El `ng-controller` directiva se usa para representar un controlador de AngularJS en la página HTML. El rol del controlador en Angular consiste en establecer el estado y el comportamiento del modelo de datos (`$scope`). Controladores no deben utilizarse para manipular el DOM directamente.

A continuación se muestra un fragmento de código que se registra un nuevo controlador. El `personApp` variable en el fragmento de código hace referencia a un módulo Angular, que se define en la línea 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

La vista que utiliza el `ng-controller` directiva asigna el nombre del controlador:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

La página muestra "Mary" y "Jane" que corresponden a la `firstName` y `lastName` propiedades adjunto a la `$scope` objeto:

![Ejemplo de controlador](angular/_static/controllers.png)

### <a name="components"></a>Componentes

[Componentes](https://docs.angularjs.org/guide/component) en Angular 1.5. x permiten la encapsulación y la capacidad de crear los elementos individuales de HTML. En Angular 1.4. x podría conseguir la misma función mediante el método .directive().

Mediante el método de .component(), desarrollo se simplifica obtengan la funcionalidad de la directiva y el controlador. Otras ventajas incluyen; aislamiento de ámbito, los procedimientos recomendados son inherentes y migración a 2 Angular se convierte en una tarea más fácil. El `<module name>.component()` llamada de función se utiliza para crear y registrar los componentes de AngularJS.

A continuación se muestra un fragmento de código que se registra un nuevo componente. El `personApp` variable en el fragmento de código hace referencia a un módulo Angular, que se define en la línea 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

La vista donde estamos mostrando el elemento HTML personalizado.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

La plantilla asociada componente utilizada:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

La página muestra "Aftab" y "Ansari" que corresponden a la `firstName` y `lastName` propiedades adjunto a la `vm` objeto:

![Ejemplo de componentes](angular/_static/components.png)

### <a name="services"></a>Servicios

[Servicios](https://docs.angularjs.org/guide/services) en AngularJS se usan normalmente para código compartido que se resume en un archivo que se puede usar durante el ciclo de vida de una aplicación Angular. Servicios haya instancias de forma diferida, lo que significa que no habrá una instancia de un servicio a menos que se usa un componente que depende del servicio. Los generadores son un ejemplo de un servicio utilizado en las aplicaciones de AngularJS. Generadores de se crean mediante el `myApp.factory()` función llamada, donde `myApp` es el módulo.

A continuación se muestra un ejemplo que muestra cómo utilizar generadores en AngularJS:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

Para llamar a este generador del controlador, pasar `personFactory` como un parámetro a la `controller` función:

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>Utilizar los servicios para comunicarse con un punto de conexión REST

A continuación se muestra un ejemplo de extremo a extremo usando servicios en AngularJS para interactuar con un punto de conexión de API Web de ASP.NET Core. El ejemplo obtiene los datos de la API Web y muestra los datos en una plantilla de vista. Comenzaremos con la vista en primer lugar:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

En esta vista, tenemos un módulo Angular denominado `PersonsApp` y llama a un controlador `personController`. Estamos usando `ng-repeat` para recorrer en iteración la lista de personas. Hacemos referencia a tres archivos JavaScript personalizados en líneas 17-19.

El *personApp.js* archivo se usa para registrar el `PersonsApp` módulo; y, la sintaxis es similar a los ejemplos anteriores. Usamos el `angular.module` función para crear una nueva instancia del módulo que se va a trabajar.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

¡Eche un vistazo a *personFactory.js*, más adelante. Estamos llamando a del módulo `factory` método para crear un generador. La línea 12 muestra el Angular integrado `$http` servicio de recuperación de información de usuarios de un servicio web.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

En *personController.js*, vamos a llamar el módulo `controller` método para crear el controlador. El `$scope` del objeto `people` propiedad se asigna a los datos devueltos por la personFactory (línea 13).

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

¡Eche un vistazo a la API de Web y el modelo subyacente. El `Person` modelo es un POCO (objeto CLR antiguos sin formato) con `Id`, `FirstName`, y `LastName` propiedades:

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

El `Person` controlador devuelve una lista con formato JSON de `Person` objetos:

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

Vamos a ver la aplicación en acción:

![Resultado del controlador de mostrar REST](angular/_static/rest-bound.png)

También puede [ver la estructura de aplicación en GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

> [!NOTE]
> Para obtener más información sobre la estructura de las aplicaciones de AngularJS, consulte [Angular Guía de John Papa de estilo](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> Para crear el módulo de AngularJS, controlador, generador, archivos de directiva y vista fácilmente, asegúrese de que desproteja Sayed Hashimi [SideWaffle el paquete de plantillas de Visual Studio](http://sidewaffle.com/). Sayed Hashimi es un director de programas en el equipo de Web de Visual Studio en Microsoft y plantillas de SideWaffle se consideran el estándar de oro. En el momento de redactar este artículo, SideWaffle está disponible para Visual Studio 2012, 2013 y 2015.

### <a name="routing-and-multiple-views"></a>Enrutamiento y varias vistas

AngularJS tiene un proveedor de ruta integrados para controlar la navegación de SPA (aplicación de página única) en función. Para trabajar con el enrutamiento de AngularJS, debe agregar el `angular-route` biblioteca mediante Bower. Puede ver en la [bower.json](#angular-bower-json) archivo al que hace referencia al principio de este artículo que se estamos ya hace referencia a él en el proyecto.

Después de instalar el paquete, agregue la referencia de script (*route.js angular*) a la vista.

Ahora veamos la aplicación de la persona que se han ido acumulando y agregar funciones de navegación a él. En primer lugar, se realizará una copia de la aplicación creando un nuevo `PeopleController` acción denominada `Spa` y su correspondiente `Spa.cshtml` vista mediante la copia de la vista de Index.cshtml en el `People` carpeta. Agregue una referencia de script a `angular-route` (vea la línea 11). Agregar un `div` marcados con el `ng-view` directiva (vea la línea 6) como marcador de posición para colocar las vistas en. Vamos a usar varias adicionales *.js* archivos que se hace referencia en las líneas 13-16.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

¡Eche un vistazo a *personModule.js* archivo para ver cómo se estamos crear instancias del módulo con el enrutamiento. Pasamos `ngRoute` como una biblioteca en el módulo. Este módulo controla el enrutamiento en nuestra aplicación.

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

El *personRoutes.js* archivo, a continuación, define las rutas basándose en el proveedor de ruta. Las líneas 4-7 definen la navegación eficazmente diciendo, cuando una dirección URL con `/persons` es solicitado, utilizar una plantilla denominada `partials/personlist` por trabajar a través de `personListController`. Líneas del 8 al 11 indican una página de detalles con un parámetro de ruta de `personId`. Si la dirección URL no coincide con uno de los modelos, Angular tiene como valor predeterminado el `/persons` vista.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

El `personlist.html` archivo es una vista parcial que contiene solo el código HTML necesario para mostrar la lista de personas.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

El controlador se define por medio del módulo `controller` funcionando en *personListController.js*.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

Si se ejecuta esta aplicación y navegue hasta el `people/spa#/persons` dirección URL, veremos:

![Vista de lista de personas](angular/_static/spa-persons.png)

Si se vaya a una página de detalles, como por ejemplo `people/spa#/persons/2`, vemos la vista parcial de detalle:

![Vista de detalle de persona](angular/_static/spa-persons-2.png)

Puede ver todo el código fuente y los archivos no se muestran en este artículo en [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

### <a name="event-handlers"></a>Controladores de eventos

Hay una serie de directivas de AngularJS que agregar capacidades de control de eventos a los elementos de entrada en el HTML DOM. A continuación se muestra una lista de los eventos que se integran en AngularJS.

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
> Puede agregar sus propios controladores de eventos mediante el [directivas personalizadas de características en AngularJS](https://docs.angularjs.org/guide/directive).

Echemos un vistazo a cómo el `ng-click` está dispuesto eventos. Cree un nuevo archivo JavaScript denominado *eventHandlerController.js*y agregue lo siguiente:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

Tenga en cuenta la nueva `sayName` funcionando en `eventHandlerController` en línea 5 anterior. Realiza todo el método para ahora se muestra una alerta de JavaScript al usuario con un mensaje de bienvenida.

La vista siguiente enlaza una función de controlador a un evento de AngularJS. La línea 9 tiene un botón en el que el `ng-click` se ha aplicado la directiva Angular. Llama a nuestro `sayName` función, que está conectado a la `$scope` objeto pasa a esta vista.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

Este ejemplo muestra que el controlador `sayName` función se invoca automáticamente cuando se hace clic en el botón.

![Click (evento)](angular/_static/events.png)

Para obtener más detalles sobre directivas de controlador de eventos integrados de AngularJS, asegúrese de encabezado hasta el [sitio Web de documentación](https://docs.angularjs.org/api/ng/directive/ngClick) de AngularJS.

## <a name="additional-resources"></a>Recursos adicionales

* [Documentos angulares](https://docs.angularjs.org)

* [Información de 2 angular](https://angular.io/)
