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
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>Marco de knockout.js MVVM en ASP.NET Core

Por [Steve Smith](http://ardalis.com)

Knockout es una biblioteca de JavaScript popular que simplifica la creación de interfaces de usuario complejas de bases de datos. Se puede utilizar por sí solo o con otras bibliotecas, como jQuery. Su objetivo principal es enlazar elementos de interfaz de usuario para un modelo de datos subyacente que se define como un objeto de JavaScript, de forma que cuando se realizan cambios en la interfaz de usuario, se actualiza el modelo y viceversa. Knockout facilita el uso de un modelo Model-View-ViewModel (MVVM) en el comportamiento del lado cliente de la aplicación web. Los dos conceptos principales uno necesario saber cuando se trabaja con la implementación de MVVM del Knockout son objetos Observables y enlaces.

## <a name="getting-started"></a>Introducción

Knockout se implementa como un solo archivo de JavaScript, por lo que la instalación y uso es muy sencillo con [bower](bower.md). Suponiendo que ya tenga [bower](bower.md) y [gulp](using-gulp.md) configurado, abra *bower.json* en el núcleo de ASP.NET del proyecto y agrega la dependencia de knockout como se muestra aquí:

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

Con esto en su lugar, puede después manualmente ejecuta bower, abra el explorador del ejecutor de tareas (en vista ‣ otras ventanas ‣ Explorador del ejecutor de tareas) y, a continuación, en tareas, haga doble clic en bower y seleccione Ejecutar. El resultado debería ser similar al siguiente:

![bower cobertura de ejecución en el explorador del ejecutor de tareas](knockout/_static/bower-knockout.png)

Ahora, si mira en el proyecto `wwwroot` carpeta, debería ver knockout instalado en la carpeta "lib".

![knockout instalado en la carpeta "lib"](knockout/_static/wwwroot-knockout.png)

Se recomienda que en su entorno de producción referencia knockout a través de una red de entrega de contenido o de red CDN, a medida que esto aumenta la probabilidad de que los usuarios ya tendrán una copia en caché del archivo y, por tanto, no tendrá que descargarlo en absoluto. Knockout está disponible en varias redes CDN, incluida la CDN de Microsoft Ajax, aquí:

[http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

Para incluir Knockout en una página que va a usar, basta con agregar un `<script>` elemento hace referencia al archivo desde donde se va a hospedarlo (con la aplicación o a través de una CDN):

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>Observables y ViewModels, enlace simple

Puede que ya esté familiarizado con utiliza JavaScript para manipular los elementos en una página web, ya sea a través del acceso directo al DOM o utilizar una biblioteca como jQuery. Normalmente, este tipo de comportamiento se logra mediante la escritura de código para establecer directamente los valores de elemento en respuesta a alguna acción del usuario. Con Knockout, un enfoque declarativo se realiza en su lugar, a través del cual los elementos de la página se enlazan a propiedades en un objeto. En lugar de escribir código para manipular los elementos DOM, las acciones del usuario simplemente interactúan con el objeto ViewModel y Knockout se encarga de asegurarse de que se sincronizan los elementos de la página.

Como ejemplo sencillo, considere la siguiente lista de página. Incluye un `<span>` elemento con un `data-bind` atributo que indica que se debe enlazar el contenido de texto Nombre del autor. Después, en un bloque de JavaScript se define un variable viewModel con una propiedad única, `authorName`, establecido a algún valor. Por último, una llamada a `ko.applyBindings` se realiza, pasando esta variable viewModel.

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

Cuando se ve en el explorador, el contenido de la <span> elemento se reemplaza con el valor de la variable de modelo de vista:

![enlace simple knockout](knockout/_static/simple-binding-screenshot.png)

Ahora tenemos trabajo simple enlace unidireccional. Tenga en cuenta que ninguna parte en el código se escribimos JavaScript para asignar un valor para el contenido del intervalo. Si desea manipular el modelo de vista, podemos realizar esto un paso más y agregar un cuadro de texto de entrada de HTML y enlazar a su valor, como para:

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

Volver a cargar la página, vemos que este valor se enlaza realmente en el cuadro de entrada:

![enlace de entrada de cobertura](knockout/_static/input-binding-screenshot.png)

Sin embargo, si se cambia el valor en el cuadro de texto, el valor correspondiente de la `<span>` no cambia el elemento. ¿A qué se debe?

El problema es que no hay nada notifique el `<span>` que necesitan actualizarse. Simplemente se actualiza el modelo de vista no es por sí mismo suficiente, a menos que las propiedades del modelo de vista se encapsulan en un tipo especial. Que debemos usar **observables** en el modelo de vista para todas las propiedades que necesitan tener los cambios se actualizan automáticamente cuando se producen. Si cambia el modelo de vista para usar `ko.observable("value")` en lugar de simplemente "value", el modelo de vista actualizará los elementos HTML que están enlazados a su valor cada vez que se produce un cambio. Tenga en cuenta que los cuadros de entrada no se actualizan su valor hasta que perderá el foco, por lo que no verá los cambios en elementos enlazados a medida que escribe.

> [!NOTE]
> Agregar compatibilidad con la actualización dinámica después de cada keypress es simplemente cuestión de agregar `valueUpdate: "afterkeydown"` a la `data-bind` contenido del atributo. También puede obtener este comportamiento mediante `data-bind="textInput: authorName"` obtener las actualizaciones de instantáneas de valores. 

Nuestro modelo de vista, después de actualizar para usar ko.observable:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Knockout admite un número de tipos diferentes de enlaces. Hasta ahora hemos visto cómo enlazar a `text` y a `value`. También puede enlazar a cualquier atributo determinado. Por ejemplo, para crear un hipervínculo con una etiqueta delimitadora, la `src` atributo puede estar limitado por el modelo de vista. Knockout también admite el enlace a las funciones. Para demostrar esto, vamos a actualizar el modelo de vista para incluir el identificador de twitter del autor y mostrar el identificador de twitter como un vínculo a la página de twitter del autor. Deberá hacerlo en tres fases.

En primer lugar, agregue el código HTML para mostrar el hipervínculo, lo que le mostraremos entre paréntesis después el nombre del autor:

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

A continuación, actualice el modelo de vista para incluir las propiedades twitterUrl y twitterAlias:

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

Tenga en cuenta que en este momento se aún no hemos actualizado el twitterUrl para ir a la dirección URL correcta para este alias de twitter, que apunta justo en twitter.com. Observe también que estamos usando una nueva función de cobertura, `computed`, para twitterUrl. Se trata de una función observable que notifiquen a los elementos de interfaz de usuario si se cambia. Sin embargo, para que pueda tener acceso a otras propiedades en el modelo de vista, es necesario cambiar cómo vamos a crear el modelo de vista, para que cada propiedad es su propia instrucción.

A continuación se muestra la declaración de viewModel revisada. Ahora se declara como una función. Tenga en cuenta que cada propiedad es ahora, su propia instrucción termina con un punto y coma. Tenga en cuenta que para tener acceso al valor de propiedad de twitterAlias, es necesario ejecutarlo, su referencia incluye ().

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

El resultado funciona según lo previsto en el explorador:

![hipervínculo de cobertura](knockout/_static/hyperlink-screenshot.png)

Knockout también admite el enlace a determinados eventos de elemento de interfaz de usuario, como el evento de clic. Esto le permite enlazar mediante declaración y fácilmente elementos de interfaz de usuario a funciones en el modelo de vista de la aplicación. Como ejemplo sencillo, podemos agregar un botón que, al hacer clic, modifica twitterAlias del autor para que sea todo en mayúsculas.

En primer lugar, agregamos el botón, haga clic en el enlace para el botón eventos y haciendo referencia al nombre de función que vamos a agregar para el modelo de vista:

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

A continuación, agregue la función en el modelo de vista y conectarlo para modificar el estado del modelo de vista. Tenga en cuenta que para establecer un nuevo valor a la propiedad twitterAlias, se llama como un método y pase el valor nuevo.

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

La ejecución de código y haga clic en el botón modifica el vínculo que aparece como se esperaba:

![poner en mayúsculas hipervínculo](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>Flujo de control

Knockout incluye enlaces que pueden realizar operaciones condicionales y bucles. Operaciones de bucle son especialmente útiles para listas de datos de enlace a listas, los menús y cuadrículas o tablas de interfaz de usuario. El enlace de foreach iterará sobre una matriz. Cuando se utiliza con una matriz observable, se actualizará automáticamente los elementos de interfaz de usuario cuando los elementos se agregan o quitan de la matriz, sin necesidad de volver a crear todos los elementos en el árbol de la interfaz de usuario. En el ejemplo siguiente se usa un viewModel nuevo que incluye una matriz de resultados del juego observable. Está enlazada a una tabla simple con dos columnas con un `foreach` de enlace de la `<tbody>` elemento. Cada `<tr>` elemento dentro de `<tbody>` se enlazará a un elemento de la colección de gameResults.

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

Tenga en cuenta que esta vez usamos ViewModel con una letra mayúscula "V" porque se espera construir con "new" (en la llamada applyBindings). Cuando se ejecuta, la página genera el siguiente resultado:

![modelo de vista de registros de cobertura](knockout/_static/record-screenshot.png)

Para demostrar que la colección observable funciona, vamos a agregar un poco más funcionalidad. Podemos incluyen la capacidad de registrar los resultados de otro juego para el modelo de vista y, a continuación, agregue un botón y alguna interfaz de usuario para trabajar con esta nueva función.  En primer lugar, vamos a crear el método addResult:

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

Este método de enlace en un botón mediante la `click` enlace:

```html
<button data-bind="click: addResult">Add New Result</button>
```

Abra la página en el explorador y haga clic en el botón de un par de veces, lo que da lugar a una nueva fila de tabla con cada clic:

![Agregar resultados](knockout/_static/record-addresult-screenshot.png)

Hay varias maneras para admitir la incorporación de nuevos registros en la interfaz de usuario, por lo general, ya sea en línea o en un formulario independiente. Se podrá modificar fácilmente la tabla para usar los cuadros de texto y listas desplegables para que todo el material es editable. Sólo debe cambiar la `<tr>` elemento tal como se muestra:

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

Tenga en cuenta que `$root` hace referencia a la raíz del modelo de vista, que es donde se exponen las opciones posibles. `$data`hace referencia a cualquier el modelo actual está dentro de un contexto determinado: en este caso, que hace referencia a un elemento individual de la matriz resultChoices, cada uno de los cuales es una cadena simple.

Con este cambio, la cuadrícula completa se convierte en modificable:

![Cuadrícula editable](knockout/_static/editable-grid-screenshot.png)

Si no estuviéramos utilizando Knockout, todo esto puede lograr mediante jQuery, pero probablemente no podría ser casi tan eficaz. Knockout realiza un seguimiento de los datos enlazados los elementos en el modelo de vista se corresponden a los elementos de interfaz de usuario y sólo actualiza los elementos que necesitan ser agregado, eliminado o actualizado. Tardaría esfuerzo significativo para lograr esto desarrollamos mediante jQuery o la manipulación directa de DOM, y aún así si, a continuación, deseamos mostrar resultados agregados (por ejemplo, un registro win-pérdida) basados en datos de la tabla, es necesario crear un bucle a través de él una vez más y analizar el Elementos HTML.  Con Knockout, mostrar el registro win-pérdida es trivial. Podemos realizar los cálculos en el modelo de vista y, a continuación, se muestran con un enlace de texto simple y un `<span>`.

Para compilar la cadena de registro de pérdida de win, podemos usar un objeto observable calculada. Tenga en cuenta que las referencias a propiedades observables en el modelo de vista debe ser llamadas a funciones, en caso contrario no recuperará el valor de la observable (es decir, `gameResults()` no `gameResults` en el código se muestra):

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

Enlazar esta función en un intervalo dentro de la `<h1>` elemento situado al principio de la página:

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

El resultado:

![Pérdida de Win](knockout/_static/record-winloss-screenshot.png)

Agregar filas o modificar el elemento seleccionado en la columna de resultados de la fila se actualizará el registro que se muestra en la parte superior de la ventana.

Además de enlace en valores, también puede utilizar casi cualquier expresión de JavaScript legal dentro de un enlace. Por ejemplo, si un elemento de interfaz de usuario solo debería aparecer bajo ciertas condiciones, por ejemplo, cuando un valor supera un cierto umbral, puede especificarlo lógicamente dentro de la expresión de enlace:

```html
<div data-bind="visible: customerValue > 100"></div>
```

Esto `<div>` sólo estará visible cuando la customerValue es superiores a 100.

## <a name="templates"></a>Plantillas

Knockout es compatible con las plantillas, para que puedan separar fácilmente la interfaz de usuario de su comportamiento, o bien cargar incrementalmente los elementos de interfaz de usuario en una aplicación de gran tamaño a petición. Podemos actualizar nuestro ejemplo anterior para hacer que cada fila su propia plantilla simplemente extrae el código HTML horizontal en una plantilla y especificando la plantilla por su nombre en la llamada de enlace de datos en `<tbody>`.

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

Knockout también admite otros motores de plantillas, como la biblioteca de jQuery.tmpl y motor de plantillas de Underscore.js.

## <a name="components"></a>Componentes

Componentes le permiten organizar y reutilizar código de interfaz de usuario, normalmente junto con los datos de modelo de vista que depende el código de interfaz de usuario. Para crear un componente, basta con especificar la plantilla y su modelo de vista y asígnele un nombre. Esto se hace llamando a `ko.components.register()`. Además de definir las plantillas y viewmodel alineado, se pueden cargar desde un archivo externo con una biblioteca como *require.js*, da lugar a un código muy limpio y eficaz.

## <a name="communicating-with-apis"></a>Comunicarse con las API

Knockout puede trabajar con los datos en formato JSON. Una forma habitual para recuperar y guardar datos usando Knockout es con jQuery, que admite la `$.getJSON()` función para recuperar datos y el `$.post()` método para enviar datos desde el explorador a un punto de conexión de API. Por supuesto, si lo prefiere de forma diferente para enviar y recibir datos JSON, Knockout funcionará con él así.

## <a name="summary"></a>Resumen

Knockout proporciona una manera sencilla y elegante para enlazar los elementos de interfaz de usuario para el estado actual de la aplicación de cliente, que se definen en un modelo de vista. Sintaxis de enlace de knockout utiliza el atributo de enlace de datos, aplicado a elementos HTML que se procesan. Knockout es capaz de procesar eficazmente y actualizar grandes conjuntos de datos mediante el seguimiento de elementos de interfaz de usuario y sólo procesando cambios en afectados los elementos. Aplicaciones de gran tamaño pueden interrumpir la lógica de la interfaz de usuario mediante plantillas y componentes, que se pueden cargar a petición desde un archivo externo. Actualmente versión 3, Knockout es una biblioteca de JavaScript estable que puede mejorar las aplicaciones web que requieren la interactividad de cliente enriquecido.
