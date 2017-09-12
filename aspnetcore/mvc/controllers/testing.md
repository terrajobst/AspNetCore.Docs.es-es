---
title: "Probar la lógica del controlador en ASP.NET Core"
author: ardalis
description: "Obtenga información acerca de cómo probar la lógica del controlador de ASP.NET Core con Moq y xUnit."
keywords: "ASP.NET Core, controlador, prueba, realizar pruebas, pruebas unitarias, pruebas unitarias, pruebas de integración, pruebas de integración, xUnit"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: dd4135ec-2b15-410c-b3fb-3d12eed4a1ac
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/testing
ms.openlocfilehash: e8a464e75dea3a0ec08c13a11888884e6bb6a4c7
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="testing-controller-logic-in-aspnet-core"></a>Probar la lógica del controlador en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Controladores en las aplicaciones de ASP.NET MVC deben ser pequeño y se centra en los problemas de la interfaz de usuario. Controladores de grandes que tratan con problemas de interfaz de usuario no son más difíciles de probar y mantener.

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>Controladores de pruebas

Los controladores son una parte fundamental de cualquier aplicación de MVC de ASP.NET Core. Por lo tanto, debe tener confianza que se comportan según lo previsto para la aplicación. Las pruebas automatizadas pueden proporcionarle esta confianza y pueden detectar errores antes de que lleguen a producción. Es importante evitar colocar innecesarias responsabilidades dentro de los controladores y asegurarse de su enfoque de pruebas solo en las responsabilidades de controlador.

Lógica de controlador debería ser mínimo y no se centra en cuestiones empresariales (por ejemplo, acceso a datos) de infraestructura o lógica. Probar la lógica del controlador, no el marco de trabajo. Prueba cómo el controlador *se comporta* en función de las entradas válidas o no es válidas. Pruebe las respuestas de controlador en función del resultado de la operación de negocio que lleva a cabo.

Responsabilidades del controlador típico:

* Comprobar `ModelState.IsValid`.
* Devolver una respuesta de error si `ModelState` no es válido.
* Recuperar una entidad de negocio de la persistencia.
* Realizar una acción en la entidad comercial.
* Guarde la entidad comercial con la persistencia.
* Devolver un apropiado `IActionResult`.

## <a name="unit-testing"></a>Pruebas unitarias

[Pruebas unitarias](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) implica probar una parte de una aplicación de forma aislada de su infraestructura y las dependencias. Cuando se prueban lógica, solo el contenido de una sola acción del controlador de pruebas unitarias, no el comportamiento de sus dependencias o el marco de trabajo. Como unidad probar las acciones de controlador, asegúrese de que solo se centran en su comportamiento. Una prueba unitaria de controlador evita cosas como [filtros](filters.md), [enrutamiento](../../fundamentals/routing.md), o [enlace de modelo](../models/model-binding.md). Centrándose en probar simplemente algo, pruebas unitarias suelen ser fáciles de escribir y rápida ejecutar. Con frecuencia se puede ejecutar un conjunto de pruebas unitarias bien escrito sin mucho sobrecarga. Sin embargo, las pruebas unitarias no detectan problemas en la interacción entre componentes, que es el propósito de [las pruebas de integración](xref:mvc/controllers/testing#integration-testing).

Si va a escribir filtros personalizados, rutas, etcetera, debería prueba unitaria de ellas, pero no como parte de las pruebas en una acción de controlador determinado. Deben probar de forma aislada.

> [!TIP]
> [Crear y ejecutar pruebas unitarias con Visual Studio](https://www.visualstudio.com/docs/code/create-and-run-unit-tests-vs).

Para mostrar las pruebas unitarias, revise el siguiente controlador. Muestra una lista de las sesiones de lluvia de ideas y permite que las sesiones que se creará con una entrada de blog de lluvia de ideas nuevas:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

El controlador es posterior a la [principio dependencias explícitas](http://deviq.com/explicit-dependencies-principle/), espera inyección de dependencia para proporcionar a una instancia de `IBrainstormSessionRepository`. Esto resulta bastante sencillo probar con el marco de objeto ficticio, al igual que [Moq](https://www.nuget.org/packages/Moq/). El `HTTP GET Index` método no tiene ningún bucle o bifurcación y solo llamadas a un método. Para probar esto `Index` método, tenemos que verificar que un `ViewResult` se devuelve, con un `ViewModel` desde el repositorio `List` método.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

El `HomeController` `HTTP POST Index` debe comprobar el método (que se muestra arriba):

* El método de acción devuelve una solicitud incorrecta `ViewResult` con los datos adecuados cuando `ModelState.IsValid` es`false`

* El `Add` se llama al método en el repositorio y un `RedirectToActionResult` se devuelve con los argumentos correctos cuando `ModelState.IsValid` es true.

Se puede probar el estado del modelo no válido mediante la adición de errores con `AddModelError` tal y como se muestra en la primera prueba siguiente.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

La primera prueba confirma cuando `ModelState` no es válido, el mismo `ViewResult` se devuelve como para una `GET` solicitud. Tenga en cuenta que la prueba no intenta pasar en un modelo no válido. Que funcionaría de todos modos ya que el enlace de modelos no se está ejecutando (aunque un [prueba de integración](xref:mvc/controllers/testing#integration-testing) utilizaría el enlace de modelos ejercicio). En este caso, el enlace de modelos no se está probando. Estas pruebas unitarias están probando solo lo que hace el código en el método de acción.

La segunda prueba comprueba que, cuando `ModelState` es válido, un nuevo `BrainstormSession` se agrega (a través del repositorio), y el método devuelve un `RedirectToActionResult` con las propiedades esperadas. Simuladas llamadas que no se llaman son normalmente omitidos, sino que realiza la llamada `Verifiable` al final de la configuración de llamada permite que se puede comprobar en la prueba. Esto se hace con la llamada a `mockRepo.Verify`, que se producirá un error la prueba si no se llamó al método esperado.

> [!NOTE]
> La biblioteca Moq utilizada en este ejemplo resulta muy sencillo mezclar simulacros comprobables o "strict", con las simulaciones no comprobable (también denominadas "débil" mocks o código auxiliar). Obtenga más información sobre [personalizar el comportamiento de simulacro con Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Otro controlador de la aplicación muestra información relacionada con una sesión de lluvia de ideas determinado. Incluye alguna lógica para tratar con valores de identificador no válido:

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

La acción de controlador tiene tres casos de prueba, uno para cada `return` instrucción:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

La aplicación expone la funcionalidad Web API (una lista de ideas asociados a una sesión de lluvia de ideas y un método para agregar nuevas ideas a una sesión):

<a name=ideas-controller></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

El `ForSession` método devuelve una lista de `IdeaDTO` tipos. Evite la devolución de las entidades de dominio empresariales directamente a través de llamadas a la API, porque con frecuencia incluyen más datos que requiere el cliente de API y acoplar innecesariamente modelo de dominio interno de la aplicación con la API expone externamente. Asignación entre las entidades de dominio y los tipos que se devolverá a través de la conexión puede realizarse manualmente (usando un LINQ `Select` tal y como se muestra aquí) o mediante una biblioteca como [AutoMapper](https://github.com/AutoMapper/AutoMapper)

Las pruebas unitarias para el `Create` y `ForSession` métodos de API:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Como se indicó anteriormente, para probar el comportamiento del método cuando `ModelState` no es válida, agrega un error de modelo para el controlador como parte de la prueba. No intente probar el enlace de validación o el modelo del modelo en las pruebas unitarias: prueba solo comportamiento de su método acción cuando se enfrente a un determinado `ModelState` valor.

La segunda prueba depende del repositorio devuelve null, por lo que está configurado el repositorio ficticio para devolver un valor nulo. No es necesario para crear una base de datos de prueba (en la memoria o de otro modo) y crear una consulta que devuelve este resultado: se puede realizar en una sola instrucción tal como se muestra.

La última prueba comprueba que el repositorio `Update` se llama al método. Tal y como se hacía anteriormente, se llama el simulacro con `Verifiable` y, a continuación, el ficticios del repositorio `Verify` método se llama para confirmar que se ejecutó el método comprobable. No es una responsabilidad de pruebas unitarias para asegurarse de que el `Update` método guarda los datos; Esto puede realizarse con una prueba de integración.

## <a name="integration-testing"></a>Pruebas de integración

[Pruebas de integración](../../testing/integration-testing.md) se realiza para garantizar correctamente juntos módulos independientes en el trabajo de la aplicación. Por lo general, todo lo que puede probar con una prueba unitaria, también puede probar con una prueba de integración, pero lo contrario no es cierto. Sin embargo, las pruebas de integración tienden a ser mucho más lentas que las pruebas unitarias. Por lo tanto, es mejor probar que se puede hacer con pruebas unitarias y usa pruebas de integración en escenarios que implican varios colaboradores.

Aunque todavía pueden ser útiles, objetos ficticios rara vez se utilizan en las pruebas de integración. En las pruebas unitarias, objetos ficticios son un método eficaz de controlar cómo deben comportarse el colaboradores fuera de la unidad que se está probando para los fines de la prueba. En una prueba de integración, colaboradores reales se usan para confirmar que el subsistema todo funciona correctamente entre sí.

### <a name="application-state"></a>Estado de la aplicación

Una consideración importante al realizar pruebas de integración se muestra cómo establecer el estado de la aplicación. Las pruebas que deba ejecutar independientes entre sí y, por lo que cada prueba debe comenzar con la aplicación en un estado conocido. Si la aplicación no usa una base de datos o que tienen cualquier persistencia, esto puede no ser un problema. Sin embargo, la mayoría de las aplicaciones reales conserva su estado a algún tipo de almacén de datos, por lo que todas las modificaciones realizadas por una prueba pudieron afectar a otra prueba a menos que se restablezca el almacén de datos. Mediante la integrada `TestServer`, resulta muy sencillo a las aplicaciones ASP.NET Core de host en nuestras pruebas de integración, pero que no necesariamente concede acceso a los datos que se va a utilizar. Si usa una base de datos real, un enfoque es que la aplicación conectarse a una base de datos de prueba, que pueden obtener acceso y asegúrese de las pruebas se restablece a un estado conocido antes de cada prueba se está ejecutando.

En esta aplicación de ejemplo, utilizo soporte de InMemoryDatabase del núcleo de Entity Framework, por lo que solo se puede conectar a él desde mi proyecto de prueba. En su lugar, exponer un `InitializeDatabase` método desde la aplicación `Startup` (clase), que llamar cuando se inicia la aplicación si se encuentra en la `Development` entorno. Mis pruebas de integración automáticamente beneficiarán de esto siempre que establezca el entorno en `Development`. No tiene que preocuparse sobre el restablecimiento de la base de datos, ya que el InMemoryDatabase se restablece cada vez que la aplicación se reinicia.

La `Startup` clase:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Verá el `GetTestSession` método que se usan con frecuencia en las pruebas de integración siguientes.

### <a name="accessing-views"></a>Obtener acceso a vistas

Cada clase de prueba de integración se configura el `TestServer` que ejecutará la aplicación de ASP.NET Core. De forma predeterminada, `TestServer` hospeda la aplicación web en la carpeta donde se está ejecutando - en este caso, la carpeta de proyecto de prueba. Por lo tanto, cuando intenta probar acciones del controlador que devuelven `ViewResult`, puede ver este error:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "none"} -->

```none
The view 'Index' was not found. The following locations were searched:
(list of locations)
```

Para corregir este problema, debe configurar la raíz del contenido del servidor, para que pueda localizar las vistas para el proyecto que se está probando. Esto se realiza mediante una llamada a `UseContentRoot` en la `TestFixture` (clase), se muestra a continuación:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

El `TestFixture` clase es responsable de configurar y crear el `TestServer`, configurando un `HttpClient` para comunicarse con el `TestServer`. Cada una de la integración de la prueba usa el `Client` propiedad para conectarse al servidor de prueba y realizar una solicitud.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

En la primera prueba anterior, el `responseString` contiene los datos reales representan HTML de la vista, que se puede inspeccionar para confirmar contiene los resultados esperados.

La segunda prueba crea un formulario POST con un nombre de sesión único y envía a la aplicación, a continuación, se comprueba que se devuelve la redirección esperada.

### <a name="api-methods"></a>Métodos de la API

Si la aplicación expone las API, lo confirmar de una buena idea ha pruebas automatizadas se ejecutan según lo previsto web. Integrado `TestServer` facilita el proceso probar las API web. Si los métodos de la API utilizan un enlace de modelo, debe comprobar siempre `ModelState.IsValid`, y las pruebas de integración son el lugar adecuado para confirmar que la validación del modelo funciona correctamente.

El siguiente conjunto de destino de pruebas la `Create` método en el [IdeasController](xref:mvc/controllers/testing#ideas-controller) clase mostrado anteriormente:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

A diferencia de las pruebas de integración de acciones que devuelve HTML vistas, los métodos de API web que devuelven resultados normalmente puede deserializados como objetos fuertemente tipados, como se muestra en la última prueba anterior. En este caso, la prueba deserializa el resultado a un `BrainstormSession` una instancia y confirma que la idea se agregó correctamente a la colección de ideas.

En este artículo encontrará ejemplos adicionales de pruebas de integración [proyecto de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).
