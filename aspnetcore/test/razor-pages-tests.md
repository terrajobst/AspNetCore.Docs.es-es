---
title: Pruebas unitarias páginas de Razor en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo crear pruebas unitarias para aplicaciones de las páginas de Razor.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: f0e47f975579dc114eaeda375028ec62696f58ed
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394740"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Pruebas unitarias páginas de Razor en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

ASP.NET Core es compatible con las pruebas unitarias de aplicaciones de las páginas de Razor. Pruebas de los datos tienen acceso a la capa (DAL) y modelos de página ayudan a garantizar:

* Partes de una aplicación de páginas de Razor funcionan independientemente y de forma conjunta como una unidad durante la construcción de la aplicación.
* Clases y métodos tienen una limitada a los ámbitos de responsabilidad.
* Existe documentación adicional sobre cómo debe comportarse la aplicación.
* Las regresiones, que son errores por actualizar el código, se encuentran durante la implementación y creación automatizada.

En este tema se supone que tiene un conocimiento básico de aplicaciones de las páginas de Razor y pruebas unitarias. Si no está familiarizado con los conceptos de pruebas o aplicaciones de las páginas de Razor, consulte los temas siguientes:

* [Introducción a las páginas de Razor](xref:razor-pages/index)
* [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Prueba unitaria de C# en .NET Core mediante pruebas de dotnet y xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

El proyecto de ejemplo se compone de dos aplicaciones:

| Aplicación         | Carpeta del proyecto                        | Descripción |
| ----------- | ------------------------------------- | ----------- |
| Aplicación de mensaje | *src/RazorPagesTestSample*            | Permite al usuario agregar, eliminar uno, elimine todo y analizar los mensajes. |
| Aplicación de prueba    | *tests/RazorPagesTestSample.Tests*    | Utilizado para la aplicación de mensajes de pruebas unitarias: Capa de acceso a datos (DAL) y el modelo de página de índice. |

Se pueden ejecutar las pruebas con las características integradas de prueba de un IDE, como [Visual Studio](https://visualstudio.microsoft.com). Si usa [Visual Studio Code](https://code.visualstudio.com/) o la línea de comandos, ejecute el siguiente comando en un símbolo del sistema en el *tests/RazorPagesTestSample.Tests* carpeta:

```console
dotnet test
```

## <a name="message-app-organization"></a>Organización de la aplicación de mensaje

La aplicación de mensaje es un sencillo sistema de mensajes de las páginas de Razor con las siguientes características:

* La página de índice de la aplicación (*Pages/index.cshtml* y *Pages/Index.cshtml.cs*) proporciona una interfaz de usuario y la página de métodos de modelo para controlar la adición, eliminación y análisis de mensajes (palabras medios por mensaje) .
* Se describe un mensaje mediante la `Message` clase (*Data/Message.cs*) con dos propiedades: `Id` (clave) y `Text` (mensaje). El `Text` propiedad es necesaria y limitada a 200 caracteres.
* Los mensajes se almacenan mediante [base de datos de Entity Framework en memoria](/ef/core/providers/in-memory/)&#8224;.
* La aplicación contiene una capa de acceso a datos (DAL) en su clase de contexto de base de datos, `AppDbContext` (*Data/AppDbContext.cs*). Los métodos de la capa DAL se marcan `virtual`, lo que permite a los métodos para su uso en las pruebas de simulación.
* Si la base de datos está vacía en el inicio de la aplicación, el almacén de mensajes se inicializa con tres mensajes. Estos *propagar mensajes* también se usan en las pruebas.

&#8224;El tema EF, [pruebas con InMemory](/ef/core/miscellaneous/testing/in-memory), se explica cómo usar una base de datos en memoria para las pruebas con MSTest. Este tema se usa el [xUnit](https://xunit.github.io/) marco de pruebas. Los conceptos de pruebas e implementaciones de prueba a través de diferentes marcos son similares pero no idénticos.

Aunque la aplicación no usa el modelo de repositorio y no es un ejemplo eficaz de la [patrón de unidades de trabajo (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages admite estos patrones de desarrollo. Para obtener más información, consulte [diseñar la capa de persistencia de infraestructura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) y [lógica del controlador de pruebas](/aspnet/core/mvc/controllers/testing) (el ejemplo implementa el modelo de repositorio).

## <a name="test-app-organization"></a>Organización de la aplicación de prueba

La aplicación de prueba es una aplicación de consola en el *tests/RazorPagesTestSample.Tests* carpeta.

| Carpeta de la aplicación de prueba | Descripción |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* contiene las pruebas unitarias para la capa DAL.</li><li>*IndexPageTests.cs* contiene las pruebas unitarias para el modelo de página de índice.</li></ul> |
| *Utilidades*     | Contiene el `TestDbContextOptions` método utilizado para crear nueva base de datos de opciones de contexto para cada prueba unitaria DAL para que la base de datos se restablece a su condición de la línea base para cada prueba. |

El marco de pruebas es [xUnit](https://xunit.github.io/). El objeto de marco de simulación es [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Pruebas unitarias de los datos de acceso a la capa (DAL)

La aplicación de mensaje tiene un DAL con cuatro métodos incluidos en el `AppDbContext` clase (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Cada método tiene una o dos pruebas unitarias de la aplicación de prueba.

| Método DAL               | Función                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Obtiene un `List<Message>` desde la base de datos ordenado por la `Text` propiedad. |
| `AddMessageAsync`        | Agrega un `Message` a la base de datos.                                          |
| `DeleteAllMessagesAsync` | Todos los elimina `Message` las entradas de la base de datos.                           |
| `DeleteMessageAsync`     | Elimina una sola `Message` desde la base de datos `Id`.                      |

Pruebas unitarias de la capa DAL requieren [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) al crear un nuevo `AppDbContext` para cada prueba. Un método para crear el `DbContextOptions` de cada prueba consiste en usar un [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

El problema con este enfoque es que cada prueba recibe la base de datos en cualquier estado de la prueba anterior lo dejó. Esto puede ser problemático cuando se intenta escribir pruebas de unidad atómica que no interfieren entre sí. Para forzar la `AppDbContext` para usar un nuevo contexto de base de datos para cada prueba, proporcione un `DbContextOptions` instancia que se basa en un nuevo proveedor de servicios. La aplicación de prueba muestra cómo hacerlo mediante su `Utilities` método de la clase `TestDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Mediante el `DbContextOptions` en la unidad DAL pruebas permite que cada prueba ejecutar de forma atómica con una instancia de base de datos actualizada:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Cada método de prueba en el `DataAccessLayerTest` clase (*UnitTests/DataAccessLayerTest.cs*) sigue un patrón similar Assert para organizar Act:

1. Organizar: La base de datos está configurada para la prueba o se define el resultado esperado.
1. actuar: Se ejecuta la prueba.
1. Aserción: Las aserciones se realizan para determinar si el resultado de la prueba es correcta.

Por ejemplo, el `DeleteMessageAsync` método es responsable de quitar un único mensaje identificado por su `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Hay dos pruebas para este método. Una prueba comprueba que el método elimina un mensaje cuando el mensaje está presente en la base de datos. Las pruebas de método que la base de datos no cambia si el mensaje `Id` para su eliminación no existe. El `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` método se muestra a continuación:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

En primer lugar, el método realiza el paso acomodar, donde realiza la preparación para el paso de Act. Los mensajes de propagación se obtuvo y mantenidos en `seedMessages`. Los mensajes de propagación se guardan en la base de datos. El mensaje con un `Id` de `1` está establecida para su eliminación. Cuando el `DeleteMessageAsync` se ejecuta el método, los mensajes esperados deben tener todos los mensajes, excepto con un `Id` de `1`. El `expectedMessages` variable representa este resultado esperado.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

El método actúa: El `DeleteMessageAsync` método se ejecuta pasando el `recId` de `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Por último, el método obtiene la `Messages` desde el contexto y lo compara con el `expectedMessages` afirmación de que los dos son iguales:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Para que compare los dos `List<Message>` son los mismos:

* Los mensajes se ordenan por `Id`.
* Se comparan pares de mensajes en el `Text` propiedad.

Un método de prueba similar, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` comprueba el resultado de intentar eliminar un mensaje que no existe. En este caso, los mensajes esperados en la base de datos deben ser iguales que el número de mensajes después de la `DeleteMessageAsync` se ejecuta el método. No debería haber ningún cambio en el contenido de la base de datos:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Pruebas unitarias de los métodos del modelo de página

Otro conjunto de pruebas unitarias es responsable de pruebas de métodos del modelo de página. En la aplicación de mensaje, los modelos de página de índice se encuentran en el `IndexModel` clase *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Método del modelo de página | Función |
| ----------------- | -------- |
| `OnGetAsync` | Obtiene los mensajes de la capa DAL para la interfaz de usuario mediante el `GetMessagesAsync` método. |
| `OnPostAddMessageAsync` | Si el `ModelState` es válido, las llamadas `AddMessageAsync` para agregar un mensaje a la base de datos. |
| `OnPostDeleteAllMessagesAsync` | Las llamadas `DeleteAllMessagesAsync` para eliminar todos los mensajes en la base de datos. |
| `OnPostDeleteMessageAsync` | Ejecuta `DeleteMessageAsync` para eliminar un mensaje con el `Id` especificado. |
| `OnPostAnalyzeMessagesAsync` | Si uno o más mensajes en la base de datos, calcula el número medio de palabras por mensaje. |

Los métodos del modelo de página se prueban utilizando siete pruebas en el `IndexPageTests` clase (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Las pruebas de usan el patrón de Act Assert organizar familiar. Estas pruebas se centran en:

* Determinar si los métodos siguen el comportamiento correcto cuando el `ModelState` no es válido.
* Confirmar los métodos generan el valor correcto `IActionResult`.
* Comprobando que se realizan correctamente las asignaciones de valor de propiedad.

Este grupo de pruebas a menudo imitar los métodos de la capa DAL para generar los datos esperados para el paso de Act donde se ejecuta un método del modelo de página. Por ejemplo, el `GetMessagesAsync` método de la `AppDbContext` se simula para producir una salida. Cuando este método ejecuta en un método del modelo de página, el simulacro devuelve el resultado. Los datos no proceden de la base de datos. Esto crea condiciones de prueba predecible y confiable para el uso de la capa DAL en las pruebas del modelo de página.

El `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` prueba muestra cómo el `GetMessagesAsync` se simula el método para el modelo de página:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Cuando el `OnGetAsync` método se ejecuta en el paso de Act, llama el modelo de página `GetMessagesAsync` método.

Paso de acción de prueba unitaria (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` modelo de página `OnGetAsync` método (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

El `GetMessagesAsync` método en la capa DAL no devuelve el resultado de esta llamada al método. La versión del método ficticio devuelve el resultado.

En el `Assert` paso, los mensajes reales (`actualMessages`) se asignan desde el `Messages` propiedad del modelo de página. También se realiza una comprobación de tipo cuando se asignan los mensajes. Los mensajes esperados y reales se comparan por sus `Text` propiedades. La prueba valida que los dos `List<Message>` instancias contienen los mismos mensajes.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Otras pruebas en este grupo crean página de objetos de modelo que incluyen la `DefaultHttpContext`, el `ModelStateDictionary`, un `ActionContext` para establecer el `PageContext`, un `ViewDataDictionary`y un `PageContext`. Estos son útiles para realizar pruebas. Por ejemplo, la aplicación de mensajes establece una `ModelState` error con `AddModelError` para comprobar que válido `PageResult` se devuelve cuando `OnPostAddMessageAsync` se ejecuta:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Recursos adicionales

* [Prueba unitaria de C# en .NET Core mediante pruebas de dotnet y xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Controladores de pruebas](xref:mvc/controllers/testing)
* [Prueba unitaria del código](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Pruebas de integración](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Introducción a xUnit.net (.NET Core/ASP.NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Guía de inicio rápido de Moq](https://github.com/Moq/moq4/wiki/Quickstart)
