---
title: Probar la lógica del controlador en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo probar la lógica del controlador en ASP.NET Core con Moq y xUnit.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2018
ms.locfileid: "41751578"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Probar la lógica del controlador en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Los controladores son una parte fundamental de cualquier aplicación ASP.NET Core MVC. Por tanto, debe tener la seguridad de que se comportan según lo previsto en la aplicación. Las pruebas automatizadas pueden darle esta seguridad, así como detectar errores antes de que lleguen a la fase producción. Es importante no asignar responsabilidades innecesarias a los controladores y procurar que las pruebas se centran únicamente en las responsabilidades del controlador.

La lógica de controlador debería ser mínima y no ir enfocada a cuestiones de infraestructura o lógica empresarial (por ejemplo, el acceso a datos). Compruebe la lógica del controlador, no el marco. Compruebe el *comportamiento* del controlador en función de las entradas válidas o no válidas. Compruebe las respuestas de controlador según el resultado de la operación empresarial que realiza.

Estas son algunas de las responsabilidades habituales de los controladores:

* Comprobar `ModelState.IsValid`
* Devolver una respuesta de error si `ModelState` no es válido
* Recuperar una entidad de negocio de la persistencia
* Llevar a cabo una acción en la entidad empresarial
* Guardar la entidad comercial para persistencia
* Devolver un `IActionResult` apropiado

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Pruebas unitarias de la lógica del controlador

Las [pruebas unitarias](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) implican probar una parte de una aplicación de forma aislada con respecto a su infraestructura y dependencias. Cuando se realizan pruebas unitarias de la lógica de controlador, solo se comprueba el contenido de una única acción, no el comportamiento de sus dependencias o del marco en sí. Cuando realice pruebas unitarias de sus acciones de controlador, asegúrese de que solo se centran en el comportamiento. Una prueba unitaria de controlador evita tener que recurrir a elementos como los [filtros](xref:mvc/controllers/filters), el [enrutamiento](xref:fundamentals/routing) o el [enlace de modelos](xref:mvc/models/model-binding). Al centrarse en comprobar solo una cosa, las pruebas unitarias suelen ser fáciles de escribir y rápidas de ejecutar. Un conjunto de pruebas unitarias bien escrito se puede ejecutar con frecuencia sin demasiada sobrecarga. Pero las pruebas unitarias no detectan problemas de interacción entre componentes, que es el propósito de las [pruebas de integración](xref:test/integration-tests).

Si va a escribir filtros personalizados y rutas, debería realizar pruebas unitarias en ellos de forma aislada, no como parte de las pruebas de una acción de controlador concreta.

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

La primera prueba confirma cuándo `ModelState` no es válido; se devuelve el mismo `ViewResult` que para una solicitud `GET`. Cabe decir que la prueba no intenta pasar un modelo no válido. Eso no funcionaría de todas formas, ya que el enlace de modelos no se está ejecutando (aunque una [prueba de integración](xref:test/integration-tests) sí usaría el enlace de modelos). En este caso concreto no estamos comprobando el enlace de modelos. Con estas pruebas unitarias solamente estamos comprobando lo que el código del método de acción hace.

La segunda prueba comprueba si, cuando `ModelState` es válido, se agrega un nuevo `BrainstormSession` (a través del repositorio) y el método devuelve un `RedirectToActionResult` con las propiedades que se esperan. Las llamadas ficticias que no se efectúan se suelen omitir, aunque llamar a `Verifiable` al final de la llamada nos permite confirmar esto en la prueba. Esto se logra con una llamada a `mockRepo.Verify`, que producirá un error en la prueba si no se ha llamado al método esperado.

> [!NOTE]
> La biblioteca Moq usada en este ejemplo nos permite mezclar fácilmente objetos ficticios comprobables (o "estrictos") con objetos ficticios no comprobables (también denominados "flexibles" o stub). Obtenga más información sobre cómo [personalizar el comportamiento de objetos ficticios con Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Otro controlador de la aplicación muestra información relacionada con una sesión de lluvia de ideas determinada. Este controlador incluye lógica para tratar los valores de identificador no válidos:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

La acción de controlador tiene tres casos que comprobar, uno por cada instrucción `return`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

La aplicación expone la funcionalidad como una API web (una lista de ideas asociadas a una sesión de lluvia de ideas y un método para agregar nuevas ideas a una sesión):

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

El método `ForSession` devuelve una lista de tipos de `IdeaDTO`. Evite devolver entidades de dominio de empresa directamente a través de llamadas API, ya que con frecuencia incluyen más datos de los que el cliente de API requiere y asocian innecesariamente el modelo de dominio interno de su aplicación con la API que se expone externamente. La asignación entre las entidades de dominio y los tipos que se van a devolver se puede realizar de forma manual (con un método `Select` de LINQ como se muestra aquí) o mediante una biblioteca como [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Estas son las pruebas unitarias de los métodos API `Create` y `ForSession`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Como se ha indicado anteriormente, si quiere comprobar el comportamiento del método cuando `ModelState` no es válido, agregue un error de modelo al controlador como parte de la prueba. No intente probar la validación del modelo o el enlace de modelos en las pruebas unitarias: céntrese tan solo en el comportamiento de su método de acción al confrontarlo con un valor de `ModelState` determinado.

La segunda prueba depende de que el repositorio devuelva null, por lo que el repositorio ficticio está configurado para devolver un valor null. No es necesario crear una base de datos de prueba (en memoria o de cualquier otro modo) ni crear una consulta que devuelva este resultado. Esto se puede realizar en una sola instrucción, tal y como se muestra.

La última prueba confirma que se llama al método `Update` del repositorio. Tal y como hicimos anteriormente, se llama al objeto ficticio con `Verifiable` y, después, se llama al método `Verify` del repositorio ficticio para confirmar que el método Verifiable se ha ejecutado. Las pruebas unitarias no se encargan de garantizar que el método `Update` guarda los datos; esto se puede realizar con una prueba de integración.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:test/integration-tests>
