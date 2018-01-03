---
title: "Unidad de páginas de Razor y pruebas de integración en ASP.NET Core"
author: guardrex
description: "Obtenga información acerca de cómo crear pruebas de unidades y de integración de aplicaciones de las páginas de Razor."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/razor-pages-testing
ms.openlocfilehash: 1ecdf010f7c283a0a08b224d570a5bc5cdf536df
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2018
---
# <a name="razor-pages-unit-and-integration-testing-in-aspnet-core"></a>Unidad de páginas de Razor y pruebas de integración en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Es compatible con ASP.NET Core unitarias y pruebas de integración de aplicaciones de las páginas de Razor. Probar la capa de acceso a datos (DAL), modelos de página y componentes de la página integrada ayuda a garantizar:

* Partes de una aplicación de páginas de Razor funcionan juntos como una unidad y de manera independiente durante la construcción de la aplicación.
* Clases y métodos han limitado ámbitos de responsabilidad.
* Documentación adicional existe en el comportamiento de la aplicación.
* Las regresiones, que son errores imperiosa actualizaciones en el código, se encuentran durante la implementación y compilación automatizada.

En este tema se da por supuesto que tiene un conocimiento básico de las aplicaciones de las páginas de Razor, pruebas unitarias y la integración de pruebas. Si no está familiarizado con las aplicaciones de las páginas de Razor o pruebas de conceptos, vea los temas siguientes:

* [Introducción a las páginas de Razor](xref:mvc/razor-pages/index)
* [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Pruebas unitarias de C# en .NET Core con xUnit y prueba de dotnet.](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Pruebas de integración](xref:testing/integration-testing)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

El proyecto de ejemplo se compone de dos aplicaciones:

| Aplicación         | Carpeta de proyecto                        | Descripción |
| ----------- | ------------------------------------- | ----------- |
| Aplicación de mensaje | *src/RazorPagesTestingSample*         | Permite a un usuario agregar, eliminar uno, elimine todos y analizar los mensajes. |
| Aplicación de prueba    | *tests/RazorPagesTestingSample.Tests* | Se usa para probar la aplicación de mensaje.<ul><li>Pruebas unitarias: capa de acceso a datos (DAL), modelo de páginas de índice</li><li>Pruebas de integración: página de índice</li></ul> |

Se pueden ejecutar las pruebas con las características integradas de prueba de un IDE, como [Visual Studio](https://www.visualstudio.com/vs/). Si usa [código de Visual Studio](https://code.visualstudio.com/) o la línea de comandos, ejecute el siguiente comando en un símbolo del sistema en el *tests/RazorPagesTestingSample.Tests* carpeta:

```console
dotnet test
```

## <a name="message-app-organization"></a>Organización de la aplicación de mensaje

La aplicación de mensaje es un sistema de mensajes de las páginas de Razor simple con las siguientes características:

* La página de índice de la aplicación (*Pages/Index.cshtml* y *Pages/Index.cshtml.cs*) proporciona una interfaz de usuario y la página métodos del modelo para controlar la adición, eliminación y análisis de mensajes (palabras medios por mensaje) .
* Describe un mensaje de la `Message` clase (*Data/Message.cs*) con dos propiedades: `Id` (clave) y `Text` (mensaje). El `Text` propiedad es necesaria y se limita a 200 caracteres.
* Los mensajes se almacenan con [base de datos de Entity Framework en memoria](/ef/core/providers/in-memory/)&#8224;.
* La aplicación contiene una capa de acceso a datos (DAL) en su clase de contexto de base de datos, `AppDbContext` (*Data/AppDbContext.cs*). Los métodos de la capa DAL se marcan `virtual`, lo que permite a los métodos para su uso en las pruebas de simulación.
* Si la base de datos está vacía en el inicio de la aplicación, el almacén de mensajes se inicializa con tres mensajes. Estos *propagado mensajes* también se utilizan en las pruebas.

&#8224; El tema EF [pruebas con InMemory](/ef/core/miscellaneous/testing/in-memory), explica cómo utilizar una base de datos en memoria para las pruebas con MSTest. Este tema se usa el [xUnit](https://xunit.github.io/) marco de pruebas. Pruebas de conceptos y las implementaciones de prueba a través de diferentes marcos de pruebas son similares pero no idénticos.

Aunque la aplicación no usa el [modelo de repositorio](http://martinfowler.com/eaaCatalog/repository.html) y no es un ejemplo efectivo de la [patrón de la unidad de trabajo (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), las páginas de Razor admite estos patrones de desarrollo. Para obtener más información, consulte [diseñar la capa de persistencia de infraestructura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementación del repositorio y patrones de unidad de trabajo en una aplicación de MVC de ASP.NET](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), y [pruebas lógica de controlador](/aspnet/core/mvc/controllers/testing) (el ejemplo implementa el modelo de repositorio).

## <a name="test-app-organization"></a>Organización de la aplicación de prueba

La aplicación de prueba es una aplicación de consola en el *tests/RazorPagesTestingSample.Tests* carpeta:

| Carpeta de la aplicación de prueba    | Descripción |
| ------------------ | ----------- |
| *IntegrationTests* | <ul><li>*IndexPageTest.cs* contiene las pruebas de integración de la página de índice.</li><li>*TestFixture.cs* crea el host de prueba para probar la aplicación de mensaje.</li></ul> |
| *UnitTests*        | <ul><li>*DataAccessLayerTest.cs* contiene las pruebas unitarias para la capa DAL.</li><li>*IndexPageTest.cs* contiene las pruebas unitarias para el modelo de páginas de índice.</li></ul> |
| *Utilidades*        | *Utilities.cs* contiene el:<ul><li>`TestingDbContextOptions`método usado para crear nueva base de datos de las opciones de contexto para cada prueba unitaria DAL para que la base de datos se restablece a su estado de línea de base para cada prueba.</li><li>`GetRequestContentAsync`método usado para preparar el `HttpClient` y contenido para las solicitudes que se envían a la aplicación de mensaje durante las pruebas de integración.</li></ul>

El marco de pruebas es [xUnit](https://xunit.github.io/). El objeto de marco de simulación es [Moq](https://github.com/moq/moq4). Pruebas de integración se realizan empleando el [Host de prueba de ASP.NET Core](xref:testing/integration-testing#the-test-host).

## <a name="unit-testing-the-data-access-layer-dal"></a>La capa de acceso a datos (DAL) de pruebas unitarias

La aplicación de mensaje tiene un DAL con cuatro métodos incluidos en el `AppDbContext` clase (*src/RazorPagesTestingSample/Data/AppDbContext.cs*). Cada método tiene una o dos pruebas unitarias de la aplicación de prueba.

| Método DAL               | Función                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Obtiene un `List<Message>` desde la base de datos ordenado por la `Text` propiedad. |
| `AddMessageAsync`        | Agrega un `Message` a la base de datos.                                          |
| `DeleteAllMessagesAsync` | Todos los elimina `Message` las entradas de la base de datos.                           |
| `DeleteMessageAsync`     | Elimina una sola `Message` desde la base de datos `Id`.                      |

Pruebas unitarias de la capa DAL requieren [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) al crear un nuevo `AppDbContext` para cada prueba. Un enfoque para crear el `DbContextOptions` para cada prueba es usar un [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

El problema con este enfoque es que cada prueba recibe la base de datos en el estado de la prueba anterior dejó. Esto puede ser problemático al intentar escribir pruebas de unidad atómica que no interfieren entre sí. Para forzar la `AppDbContext` para utilizar un nuevo contexto de base de datos para cada prueba, proporcione un `DbContextOptions` instancia que se basa en un nuevo proveedor de servicios. La aplicación de prueba muestra cómo hacer esto con su `Utilities` método de la clase `TestingDbContextOptions` (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Mediante el `DbContextOptions` en la unidad de la capa DAL pruebas permite que cada prueba para ejecutarse de forma atómica con una una instancia nueva de la base de datos:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Cada método de prueba en el `DataAccessLayerTest` clase (*UnitTests/DataAccessLayerTest.cs*) sigue un patrón similar Assert para organizar Act:

1. Organizar: La base de datos está configurado para la prueba o se define el resultado esperado.
1. Acción: En la que se ejecuta la prueba.
1. Aserción: Las aserciones se realizan para determinar si el resultado de la prueba es correcta.

Por ejemplo, el `DeleteMessageAsync` método es responsable de quitar un único mensaje identificado por su `Id` (*src/RazorPagesTestingSample/Data/AppDbContext.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

Hay dos pruebas para este método. Una prueba comprueba que el método elimina un mensaje cuando el mensaje está presente en la base de datos. Las demás pruebas de método que la base de datos no cambia si el mensaje `Id` para su eliminación no existe. El `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` método se muestra a continuación:

[!code-csharp[Main](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

En primer lugar, el método realiza el paso de organización, donde realiza la preparación para el paso de acción. Los mensajes de propagación se obtienen y mantienen en `seedMessages`. Los mensajes de propagación se guardan en la base de datos. El mensaje con un `Id` de `1` está establecido para su eliminación. Cuando el `DeleteMessageAsync` método se ejecuta, los mensajes esperados deben tener todos los mensajes excepto el con una `Id` de `1`. El `expectedMessages` variable representa este resultado esperado.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

El método actúa: el `DeleteMessageAsync` método se ejecuta pasando el `recId` de `1`:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Por último, el método obtiene la `Messages` desde el contexto y lo compara con el `expectedMessages` validar que los dos son iguales:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Para que compare los dos `List<Message>` son los mismos:

* Los mensajes se ordenan por `Id`.
* Se comparan los pares de mensajes en el `Text` propiedad.

Un método de prueba similar, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` comprueba el resultado de intentar eliminar un mensaje que no existe. En este caso, los mensajes esperados en la base de datos deben ser iguales a los mensajes reales después de la `DeleteMessageAsync` se ejecuta el método. No debería haber ningún cambio en el contenido de la base de datos:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a>Los métodos del modelo de página de pruebas unitarias

Otro conjunto de pruebas unitarias es responsable de probar los métodos del modelo de página. En la aplicación de mensaje, los modelos de página de índice se encuentran en el `IndexModel` clase *src/RazorPagesTestingSample/Pages/Index.cshtml.cs*.

| Método del modelo de página | Función |
| ----------------- | -------- | 
| `OnGetAsync` | Obtiene los mensajes de la capa DAL para la interfaz de usuario con el `GetMessagesAsync` método. |
| `OnPostAddMessageAsync` | Si el `ModelState` es válido, las llamadas `AddMessageAsync` para agregar un mensaje a la base de datos. | 
| `OnPostDeleteAllMessagesAsync` | Llamadas `DeleteAllMessagesAsync` para eliminar todos los mensajes en la base de datos. |
| `OnPostDeleteMessageAsync` | Ejecuta `DeleteMessageAsync` para eliminar un mensaje con el `Id` especificado. |
| `OnPostAnalyzeMessagesAsync` | Si uno o más mensajes en la base de datos, calcula el número medio de palabras por mensaje. |

Los métodos del modelo de página se prueban utilizando siete pruebas en la `IndexPageTest` clase (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*). Las pruebas usan el patrón de Act Assert organizar familiarizado. Estas pruebas se centran en:

* Determinar si los métodos siguen el comportamiento correcto cuando el `ModelState` no es válido.
* Confirmar los métodos generan el valor correcto `IActionResult`.
* Comprobando que las asignaciones de valor de propiedad se realizan correctamente.

Este grupo de pruebas a menudo simular los métodos de la capa DAL para generar datos esperado para el paso de acción que se ejecuta un método de modelo de página. Por ejemplo, el `GetMessagesAsync` método de la `AppDbContext` simulada para generar un resultado. Cuando un método de modelo de la página ejecuta este método, el simulacro devuelve el resultado. Los datos no proceden de la base de datos. Esto crea condiciones de prueba predecible y confiable para el uso de la capa DAL en las pruebas de modelo de página.

El `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` prueba muestra cómo el `GetMessagesAsync` método es simulado para el modelo de páginas:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

Cuando el `OnGetAsync` método se ejecuta en el paso de acción, llama al modelo de página `GetMessagesAsync` método.

Paso de acción de prueba unitaria (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

`IndexPage`modelo de páginas `OnGetAsync` método (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

El `GetMessagesAsync` método en la capa DAL no devuelve el resultado de llamar a este método. La versión del método simulada devuelve el resultado.

En el `Assert` paso, los mensajes reales (`actualMessages`) se asignan a partir del `Messages` propiedad del modelo de página. También se realiza una comprobación de tipo cuando se asignen los mensajes. Los mensajes esperados y reales se comparan sus `Text` propiedades. La prueba valida que los dos `List<Message>` instancias contienen los mismos mensajes.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

Otras pruebas en este grupo crean página de objetos del modelo que incluyen la `DefaultHttpContext`, el `ModelStateDictionary`, `ActionContext` para establecer el `PageContext`, un `ViewDataDictionary`y un `PageContext`. Estos son útiles para realizar las pruebas. Por ejemplo, la aplicación de mensaje establece una `ModelState` error con `AddModelError` para comprobar que válido `PageResult` se devuelve cuando `OnPostAddMessageAsync` se ejecuta:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a>La aplicación de pruebas de integración

La integración de la prueba se centran en pruebas que funcionan conjuntamente los componentes de la aplicación. Pruebas de integración se realizan empleando el [Host de prueba de ASP.NET Core](xref:testing/integration-testing#the-test-host). Se ha probado el procesamiento de ciclo de vida completo de solicitudes y respuestas. Estas pruebas de aserción que la página genera el código de estado correcto y `Location` encabezado, si establece.

Un ejemplo del ejemplo de pruebas de integración, comprueba el resultado de la página de índice de la aplicación de mensaje de solicitud (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

No hay ningún paso de organización. El `GetAsync` método se llama en el `HttpClient` para enviar una solicitud GET al extremo. La prueba valida que el resultado es un código de estado 200 OK.

Cualquier solicitud POST a la aplicación de mensaje debe cumplir la comprobación de antiforgery que se convierte automáticamente en la aplicación [antiforgery sistema de protección de datos](xref:security/data-protection/introduction). Para organizar de solicitud POST de una prueba, la aplicación de prueba debe:

1. Realice una solicitud de la página.
1. Analizar la cookie antiforgery y el token de validación de solicitud de la respuesta.
1. Asegúrese de la solicitud POST con la validación de solicitud y cookie antiforgery símbolo (token) en su lugar.

El `Post_AddMessageHandler_ReturnsRedirectToRoot` método de prueba:

* Prepara un mensaje y la `HttpClient`.
* Realiza una solicitud POST a la aplicación.
* Comprueba que la respuesta es una redirección a la página de índice.

`Post_AddMessageHandler_ReturnsRedirectToRoot `método (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

El `GetRequestContentAsync` método de utilidad administra la preparación del cliente con la cookie antiforgery y el token de comprobación de la solicitud. Tenga en cuenta cómo el método recibe una `IDictionary` que permite que el método de prueba que realiza la llamada para pasar los datos de la solicitud codificar junto con el token de comprobación de solicitud (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

Pruebas de integración también pueden pasar datos incorrectos a la aplicación para probar el comportamiento de respuesta de la aplicación. La aplicación de mensaje limita la longitud del mensaje a 200 caracteres (*src/RazorPagesTestingSample/Data/Message.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

El `Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong` probar `Message` pasa explícitamente en texto con 201 caracteres "X". Esto da como resultado un `ModelState` error. La publicación no se redirige a la página de índice. Devuelve un 200 OK con un `null` `Location` encabezado (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a>Vea también

* [Pruebas unitarias de C# en .NET Core con xUnit y prueba de dotnet.](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Pruebas de integración](xref:testing/integration-testing)
* [Pruebas de controladores](xref:mvc/controllers/testing)
* [El código de prueba unitaria](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [xUnit.net](https://xunit.github.io/)
* [Introducción a xUnit.net (.NET Core/ASP.NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Inicio rápido de Moq](https://github.com/Moq/moq4/wiki/Quickstart)
