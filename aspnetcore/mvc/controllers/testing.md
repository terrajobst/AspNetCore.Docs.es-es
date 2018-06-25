---
title: Probar la lógica del controlador en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo probar la lógica del controlador en ASP.NET Core con Moq y xUnit.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: fc5f10b4d5947a6af114bf00f8b1d955b083a44d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273928"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Probar la lógica del controlador en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Los controladores de las aplicaciones ASP.NET MVC deben ser de tamaño reducido e ir enfocados a abordar problemas de la interfaz de usuario. Los controladores de gran tamaño que tratan problemas no relacionados con la interfaz de usuario son bastante más difíciles de probar y mantener.

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>Probar los controladores

Los controladores son una parte fundamental de cualquier aplicación ASP.NET Core MVC. Por tanto, debe tener la seguridad de que se comportan según lo previsto en la aplicación. Las pruebas automatizadas pueden darle esta seguridad, así como detectar errores antes de que lleguen a la fase producción. Es importante no asignar responsabilidades innecesarias a los controladores y procurar que las pruebas se centran únicamente en las responsabilidades del controlador.

La lógica de controlador debería ser mínima y no ir enfocada a cuestiones de infraestructura o lógica empresarial (por ejemplo, el acceso a datos). Compruebe la lógica del controlador, no el marco. Compruebe el *comportamiento* del controlador en función de las entradas válidas o no válidas. Compruebe las respuestas de controlador según el resultado de la operación empresarial que realiza.

Estas son algunas de las responsabilidades habituales de los controladores:

* Comprobar `ModelState.IsValid`
* Devolver una respuesta de error si `ModelState` no es válido
* Recuperar una entidad de negocio de la persistencia
* Llevar a cabo una acción en la entidad empresarial
* Guardar la entidad comercial para persistencia
* Devolver un `IActionResult` apropiado

## <a name="unit-testing"></a>Pruebas unitarias

Las [pruebas unitarias](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) conllevan probar una parte de una aplicación de forma aislada con respecto a su infraestructura y dependencias. Cuando se realizan pruebas unitarias de la lógica de controlador, solo se comprueba el contenido de una única acción, no el comportamiento de sus dependencias o del marco en sí. Cuando realice pruebas unitarias de sus acciones de controlador, asegúrese de que solo se centran en el comportamiento. Una prueba unitaria de controlador evita tener que recurrir a elementos como los [filtros](filters.md), el [enrutamiento](../../fundamentals/routing.md) o el [enlace de modelos](../models/model-binding.md). Al centrarse en comprobar solo una cosa, las pruebas unitarias suelen ser fáciles de escribir y rápidas de ejecutar. Un conjunto de pruebas unitarias bien escrito se puede ejecutar con frecuencia sin demasiada sobrecarga. Pero las pruebas unitarias no detectan problemas de interacción entre componentes, que es el propósito de las [pruebas de integración](xref:mvc/controllers/testing#integration-testing).

Si está escribiendo filtros personalizados, rutas, etc., debería realizar pruebas unitarias en ellos, pero no como parte de las comprobaciones de una acción de controlador determinada, sino de forma aislada.

> [!TIP]
> [Cree y ejecute pruebas unitarias con Visual Studio](/visualstudio/test/unit-test-your-code).

Para explicar las pruebas unitarias, revisaremos el siguiente controlador. Muestra una lista de sesiones de lluvia de ideas y permite crear nuevas sesiones de lluvia de ideas con un método POST:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

El controlador sigue el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/), de modo que espera que la inserción de dependencias le proporcione una instancia de `IBrainstormSessionRepository`. Esto es bastante sencillo de comprobar si se usa un marco de objeto ficticio, como [Moq](https://www.nuget.org/packages/Moq/). El método `HTTP GET Index` no tiene bucles ni bifurcaciones y solamente llama a un método. Para probar este método `Index`, tenemos que confirmar que se devuelve un `ViewResult`, con un `ViewModel` del método `List` del repositorio.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

El método `HomeController` `HTTP POST Index` (mostrado arriba) debe comprobar lo siguiente:

* El método de acción devuelve un `ViewResult` de solicitud incorrecta con los datos adecuados cuando `ModelState.IsValid` es `false`.

* Se llama al método `Add` en el repositorio y se devuelve un `RedirectToActionResult` con los argumentos correctos cuando `ModelState.IsValid` es true.

El estado de modelo no válido se puede comprobar introduciendo errores con `AddModelError`, como se muestra en la primera prueba de abajo.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

La primera prueba confirma cuándo `ModelState` no es válido; se devuelve el mismo `ViewResult` que para una solicitud `GET`. Cabe decir que la prueba no intenta pasar un modelo no válido. Eso no funcionaría de todas formas, ya que el enlace de modelos no se está ejecutando (aunque una [prueba de integración](xref:mvc/controllers/testing#integration-testing) sí usaría el enlace de modelos). En este caso concreto no estamos comprobando el enlace de modelos. Con estas pruebas unitarias solamente estamos comprobando lo que el código del método de acción hace.

La segunda prueba comprueba si, cuando `ModelState` es válido, se agrega un nuevo `BrainstormSession` (a través del repositorio) y el método devuelve un `RedirectToActionResult` con las propiedades que se esperan. Las llamadas ficticias que no se efectúan se suelen omitir, aunque llamar a `Verifiable` al final de la llamada nos permite confirmar esto en la prueba. Esto se logra con una llamada a `mockRepo.Verify`, que producirá un error en la prueba si no se ha llamado al método esperado.

> [!NOTE]
> La biblioteca Moq usada en este ejemplo nos permite mezclar fácilmente objetos ficticios comprobables (o "estrictos") con objetos ficticios no comprobables (también denominados "flexibles" o stub). Obtenga más información sobre cómo [personalizar el comportamiento de objetos ficticios con Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Otro controlador de la aplicación muestra información relacionada con una sesión de lluvia de ideas determinada. Este controlador incluye lógica para tratar los valores de identificador no válidos:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

La acción de controlador tiene tres casos que comprobar, uno por cada instrucción `return`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

La aplicación expone la funcionalidad como una API web (una lista de ideas asociadas a una sesión de lluvia de ideas y un método para agregar nuevas ideas a una sesión):

<a name="ideas-controller"></a>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

El método `ForSession` devuelve una lista de tipos de `IdeaDTO`. Evite devolver entidades de dominio de empresa directamente a través de llamadas API, ya que con frecuencia incluyen más datos de los que el cliente de API requiere y asocian innecesariamente el modelo de dominio interno de su aplicación con la API que se expone externamente. La asignación entre las entidades de dominio y los tipos que se van a devolver se puede realizar manualmente (usando un método `Select` de LINQ, tal y como se muestra aquí) o por medio de una biblioteca como [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Estas son las pruebas unitarias de los métodos API `Create` y `ForSession`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Como se ha indicado anteriormente, si quiere comprobar el comportamiento del método cuando `ModelState` no es válido, agregue un error de modelo al controlador como parte de la prueba. No intente probar la validación del modelo o el enlace de modelos en las pruebas unitarias: céntrese tan solo en el comportamiento de su método de acción al confrontarlo con un valor de `ModelState` determinado.

La segunda prueba depende de que el repositorio devuelva null, por lo que el repositorio ficticio está configurado para devolver un valor null. No es necesario crear una base de datos de prueba (en memoria o de cualquier otro modo) ni crear una consulta que devuelva este resultado. Esto se puede realizar en una sola instrucción, tal y como se muestra.

La última prueba confirma que se llama al método `Update` del repositorio. Tal y como hicimos anteriormente, se llama al objeto ficticio con `Verifiable` y, después, se llama al método `Verify` del repositorio ficticio para confirmar que el método Verifiable se ha ejecutado. Las pruebas unitarias no se encargan de garantizar que el método `Update` guarda los datos; esto se puede realizar con una prueba de integración.

## <a name="integration-testing"></a>Pruebas de integración

Las [pruebas de integración](xref:test/integration-tests) se realizan para garantizar que distintos módulos independientes dentro de la aplicación funcionan correctamente juntos. Por lo general, todo lo que se puede comprobar con una prueba unitaria también se puede comprobar con una prueba de integración, pero no a la inversa. Pero las pruebas de integración suelen ser mucho más lentas que las unitarias. Por tanto, lo mejor es comprobar todo lo que sea factible con las pruebas unitarias y recurrir a las pruebas de integración en los casos en los que existan varios colaboradores.

Los objetos ficticios rara vez se usan en las pruebas de integración, aunque pueden seguir siendo de utilidad. En las pruebas unitarias, los objetos ficticios constituyen un método eficaz de controlar el modo en que los colaboradores fuera de la unidad que se está probando deben comportarse según los propósitos de la prueba. En una prueba de integración se usan colaboradores reales para confirmar que todo el subsistema funciona correctamente en conjunto.

### <a name="application-state"></a>Estado de la aplicación

Una consideración importante al realizar pruebas de integración es cómo establecer el estado de la aplicación. Las pruebas se deben ejecutar de manera independiente entre sí, por lo que cada prueba debe comenzar con la aplicación en un estado conocido. Que la aplicación no use una base de datos o tenga algún tipo de persistencia no debe ser un problema. Pero la mayoría de las aplicaciones reales almacenan su estado en algún tipo de almacén de datos, de modo que cualquier modificación que se realice a raíz de una prueba puede repercutir en otra, a menos que se restablezca el almacén de datos. Si se usa el método integrado `TestServer`, será muy fácil hospedar aplicaciones ASP.NET Core en nuestras pruebas de integración, pero esto no da acceso necesariamente a los datos que se van a usar. Si se usa una base de datos real, un método consiste en conectar la aplicación a una base de datos de prueba, a la que las pruebas pueden obtener acceso, y confirmar que está restablecida en un estado conocido antes de que cada prueba se ejecute.

En esta aplicación de ejemplo, uso la base de datos InMemoryDatabase de Entity Framework Core, por lo que no puedo simplemente conectarme a ella desde mi proyecto de prueba. En su lugar, expondré un método `InitializeDatabase` desde la clase `Startup` de la aplicación, y llamaré a ese método cuando la aplicación se inicie si está en el entorno `Development`. Mis pruebas de integración sacarán partido de esto automáticamente siempre que tengan el entorno establecido en `Development`. No tiene que preocuparse de restablecer la base de datos, ya que InMemoryDatabase se restablece cada vez que la aplicación se reinicia.

La clase `Startup`:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Verá que el método `GetTestSession` se usa con bastante asiduidad en las siguientes pruebas de integración.

### <a name="accessing-views"></a>Acceso a las vistas

En cada clase de prueba de integración se configura el método `TestServer` que ejecutará la aplicación de ASP.NET Core. `TestServer` hospeda la aplicación web de forma predeterminada en la carpeta donde se está ejecutando (en este caso, la carpeta del proyecto de prueba). Por tanto, si intenta probar las acciones del controlador que devuelven `ViewResult`, es posible que aparezca este error:

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

Para corregir este problema, debe configurar la raíz del contenido del servidor de forma que pueda localizar las vistas del proyecto que se está comprobando. Esto se consigue con una llamada a `UseContentRoot` en la clase `TestFixture`, como se aprecia aquí:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

La clase `TestFixture` se encarga de configurar y crear el método `TestServer`, que configura un `HttpClient` para comunicarse con dicho método `TestServer`. En cada una de las pruebas de integración se usa la propiedad `Client` para conectarse al servidor de prueba y realizar una solicitud.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

En la primera prueba de arriba, `responseString` contiene el HTML realmente presentado de la vista, que se puede revisar para confirmar que contiene los resultados esperados.

La segunda prueba crea un formulario POST con un nombre de sesión único y lo envía a la aplicación para, seguidamente, confirmar que se devuelve la redirección prevista.

### <a name="api-methods"></a>Métodos de API

Si la aplicación expone API web, conviene confirmar que se ejecutan según lo previsto por medio de pruebas automatizadas. El método integrado `TestServer` permite comprobar API web de forma muy sencilla. Si los métodos de API usan el enlace de modelos, deberá comprobar siempre el factor `ModelState.IsValid` y, en este sentido, las pruebas de integración son el lugar adecuado para confirmar que la validación del modelo funciona correctamente.

El siguiente conjunto de pruebas tiene como destino el método `Create` en la clase [IdeasController](xref:mvc/controllers/testing#ideas-controller) de arriba:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

A diferencia de las pruebas de integración de acciones que devuelven vistas HTML, los métodos de API web que devuelven resultados se suelen poder deserializar como objetos fuertemente tipados, tal y como arroja la última prueba mostrada arriba. En este caso, la prueba deserializa el resultado en una instancia de `BrainstormSession` y confirma que la idea se agregó correctamente a la colección de ideas.

En el [proyecto de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) de este artículo encontrará más ejemplos de pruebas de integración.
