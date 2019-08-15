---
title: Razor Pages pruebas unitarias en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo crear pruebas unitarias para aplicaciones Razor Pages.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: 35feb5dd95fa79ceca7ff03523cef30d29ccbdd3
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022573"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Razor Pages pruebas unitarias en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core admite pruebas unitarias de aplicaciones de Razor Pages. Las pruebas de la capa de acceso a datos (DAL) y los modelos de página ayudan a garantizar:

* Las partes de una aplicación Razor Pages funcionan de forma independiente y conjunta como una unidad durante la construcción de la aplicación.
* Las clases y los métodos tienen ámbitos limitados de responsabilidad.
* Existe documentación adicional sobre el comportamiento de la aplicación.
* Las regresiones, que son errores que se producen al actualizar el código, se encuentran durante la creación e implementación automatizadas.

En este tema se supone que tiene conocimientos básicos de Razor Pages aplicaciones y pruebas unitarias. Si no está familiarizado con las aplicaciones de Razor Pages o los conceptos de prueba, vea los temas siguientes:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Pruebas C# unitarias en .net Core con pruebas de dotnet y xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

El proyecto de ejemplo se compone de dos aplicaciones:

| Aplicación         | Carpeta de proyecto                     | DESCRIPCIÓN |
| ----------- | ---------------------------------- | ----------- |
| Aplicación de mensaje | *src/RazorPagesTestSample*         | Permite que un usuario agregue un mensaje, elimine un mensaje, elimine todos los mensajes y analice los mensajes (busque el número promedio de palabras por mensaje). |
| Aplicación de prueba    | *tests/RazorPagesTestSample.Tests* | Se usa para probar unitarias el modelo de página de índice y la capa DAL de la aplicación de mensaje. |

Las pruebas se pueden ejecutar con las características de prueba integradas de un IDE, como [Visual Studio](/visualstudio/test/unit-test-your-code) o [Visual Studio para Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). Si usa [Visual Studio Code](https://code.visualstudio.com/) o la línea de comandos, ejecute el siguiente comando en un símbolo del sistema en la carpeta *tests/RazorPagesTestSample. tests* :

```console
dotnet test
```

## <a name="message-app-organization"></a>Organización de aplicaciones de mensajes

La aplicación de mensaje es un sistema de mensajes Razor Pages con las siguientes características:

* La página de índice de la aplicación (*pages/index. cshtml* y *pages/index. cshtml. CS*) proporciona un método de interfaz de usuario y un modelo de página para controlar la adición, eliminación y análisis de mensajes (buscar el número promedio de palabras por mensaje).
* La `Message` clase (*Data/Message. CS*) describe un mensaje con dos propiedades: `Id` (Key) y `Text` (Message). La `Text` propiedad es necesaria y está limitada a 200 caracteres.
* Los mensajes se almacenan mediante&#8224; [Entity Framework base de datos en memoria](/ef/core/providers/in-memory/).
* La aplicación contiene una capa Dal en su clase de contexto `AppDbContext` de base de datos, (*Data/AppDbContext. CS*). Los métodos Dal están marcados `virtual`, lo que permite simular los métodos para su uso en las pruebas.
* Si la base de datos está vacía al iniciarse la aplicación, el almacén de mensajes se inicializa con tres mensajes. Estos *mensajes inicializados* también se usan en las pruebas.

&#8224;En el tema EF, [Test with inmemory](/ef/core/miscellaneous/testing/in-memory), se explica cómo usar una base de datos en memoria para las pruebas con MSTest. En este tema se usa el marco de pruebas de [xUnit](https://xunit.github.io/) . Los conceptos de prueba y las implementaciones de prueba en diferentes marcos de pruebas son similares, pero no idénticos.

Aunque la aplicación de ejemplo no usa el patrón de repositorio y no es un ejemplo eficaz del [patrón de unidad de trabajo (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor pages admite estos patrones de desarrollo. Para obtener más información, vea [diseñar el nivel de persistencia de la infraestructura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) y <xref:mvc/controllers/testing> (el ejemplo implementa el patrón del repositorio).

## <a name="test-app-organization"></a>Probar la organización de la aplicación

La aplicación de prueba es una aplicación de consola dentro de la carpeta *tests/RazorPagesTestSample. tests* .

| Carpeta de aplicación de prueba | DESCRIPCIÓN |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.CS* contiene las pruebas unitarias para la capa dal.</li><li>*IndexPageTests.CS* contiene las pruebas unitarias para el modelo de página de índice.</li></ul> |
| *Utilidad*     | Contiene el `TestDbContextOptions` método utilizado para crear nuevas opciones de contexto de base de datos para cada prueba unitaria de la capa Dal, de modo que la base de datos se restablezca a su condición de línea base para cada prueba. |

El marco de pruebas es [xUnit](https://xunit.github.io/). El marco de trabajo ficticio de objetos es [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Pruebas unitarias de la capa de acceso a datos (DAL)

La aplicación de mensaje tiene un Dal con cuatro métodos contenidos en `AppDbContext` la clase (*src/RazorPagesTestSample/Data/AppDbContext. CS*). Cada método tiene una o dos pruebas unitarias en la aplicación de prueba.

| DAL (método)               | Función                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Obtiene de la base de datos ordenada por la `Text` propiedad. `List<Message>` |
| `AddMessageAsync`        | Agrega un `Message` a la base de datos.                                          |
| `DeleteAllMessagesAsync` | Elimina todas `Message` las entradas de la base de datos.                           |
| `DeleteMessageAsync`     | Elimina un único `Message` de la base de datos `Id`por.                      |

Las pruebas unitarias de la <xref:Microsoft.EntityFrameworkCore.DbContextOptions> capa Dal requieren al `AppDbContext` crear un nuevo para cada prueba. Un enfoque para crear el `DbContextOptions` para cada prueba es usar un: <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

El problema de este enfoque es que cada prueba recibe la base de datos en el estado que la prueba anterior la dejó. Esto puede ser problemático al intentar escribir pruebas unitarias atómicas que no interfieren entre sí. Para forzar `AppDbContext` a que use un nuevo contexto de base de datos para cada `DbContextOptions` prueba, proporcione una instancia de basada en un nuevo proveedor de servicios. La aplicación de prueba muestra cómo hacerlo mediante su `Utilities` método `TestDbContextOptions` de clase (*tests/RazorPagesTestSample. tests/Utilities/Utilities. CS*):

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

El `DbContextOptions` uso de en las pruebas unitarias de dal permite que cada prueba se ejecute de forma atómica con una nueva instancia de base de datos:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Cada método de prueba de `DataAccessLayerTest` la clase (*UnitTests/DataAccessLayerTest. CS*) sigue un patrón de Arrange-Act-Assert similar:

1. Orden La base de datos está configurada para la prueba y/o se ha definido el resultado esperado.
1. LEDs La prueba se ejecuta.
1. Declarar Se realizan aserciones para determinar si el resultado de la prueba es un éxito.

Por ejemplo, el `DeleteMessageAsync` método es responsable de quitar un solo mensaje identificado por su `Id` (*src/RazorPagesTestSample/Data/AppDbContext. CS*):

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Hay dos pruebas para este método. Una prueba comprueba que el método elimina un mensaje cuando el mensaje está presente en la base de datos. El otro método comprueba que la base de datos no cambia si `Id` no existe el mensaje para su eliminación. El `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` método se muestra a continuación:

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

En primer lugar, el método realiza el paso de organización, donde tiene lugar la preparación para el paso de Act. Los mensajes de propagación se obtienen y se mantienen `seedMessages`en. Los mensajes de propagación se guardan en la base de datos. El mensaje con un `Id` de `1` se establece para su eliminación. Cuando se `DeleteMessageAsync` ejecuta el método, los mensajes esperados deben tener todos los mensajes excepto el que tiene un `Id` de `1`. La `expectedMessages` variable representa este resultado esperado.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

El método actúa: El `DeleteMessageAsync` método se ejecuta pasando el `recId` de `1`:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Por último, el método obtiene el `Messages` del contexto y lo compara con la `expectedMessages` aserción de que los dos son iguales:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Para comparar que los dos `List<Message>` son los mismos:

* Los mensajes se ordenan `Id`por.
* Los pares de mensajes se comparan en la `Text` propiedad.

Un método `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` de prueba similar comprueba el resultado de intentar eliminar un mensaje que no existe. En este caso, los mensajes esperados en la base de datos deben ser iguales a los mensajes `DeleteMessageAsync` reales después de ejecutar el método. No debe haber ningún cambio en el contenido de la base de datos:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Pruebas unitarias de los métodos del modelo de página

Otro conjunto de pruebas unitarias es responsable de las pruebas de los métodos del modelo de página. En la aplicación de mensaje, los modelos de página de índice se `IndexModel` encuentran en la clase en *src/RazorPagesTestSample/pages/index. cshtml. CS*.

| Método del modelo de página | Función |
| ----------------- | -------- |
| `OnGetAsync` | Obtiene los mensajes de la capa Dal para la interfaz de usuario `GetMessagesAsync` mediante el método. |
| `OnPostAddMessageAsync` | Si el [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) es válido, llama `AddMessageAsync` a para agregar un mensaje a la base de datos. |
| `OnPostDeleteAllMessagesAsync` | Llama `DeleteAllMessagesAsync` a para eliminar todos los mensajes de la base de datos. |
| `OnPostDeleteMessageAsync` | Ejecuta para eliminar un mensaje con el `Id` especificado. `DeleteMessageAsync` |
| `OnPostAnalyzeMessagesAsync` | Si uno o más mensajes están en la base de datos, calcula el número promedio de palabras por mensaje. |

Los métodos del modelo de página se prueban con siete `IndexPageTests` pruebas en la clase (*tests/RazorPagesTestSample. tests/UnitTests/IndexPageTests. CS*). Las pruebas usan el conocido patrón Arrange-Assert-Act. Estas pruebas se centran en:

* Determinar si los métodos siguen el comportamiento correcto cuando [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) no es válido.
* La confirmación de los métodos produce el correcto <xref:Microsoft.AspNetCore.Mvc.IActionResult>.
* Comprobando que las asignaciones de valores de propiedad se realizan correctamente.

Este grupo de pruebas suele simular los métodos de la capa DAL para generar los datos esperados para el paso de la acción en el que se ejecuta un método de modelo de página. Por ejemplo, el `GetMessagesAsync` método `AppDbContext` de se simula para generar la salida. Cuando un método de modelo de página ejecuta este método, el simulacro devuelve el resultado. Los datos no provienen de la base de datos. Esto crea condiciones de prueba de confianza y predecible para usar la capa DAL en las pruebas del modelo de página.

La `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` prueba muestra cómo se `GetMessagesAsync` simula el método para el modelo de página:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Cuando el `OnGetAsync` método se ejecuta en el paso de Act, llama al método del modelo `GetMessagesAsync` de página.

Paso de acción de prueba unitaria (tests */RazorPagesTestSample. tests/UnitTests/IndexPageTests. CS*):

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`método del modelo `OnGetAsync` de página (*src/RazorPagesTestSample/pages/index. cshtml. CS*):

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

El `GetMessagesAsync` método de la capa Dal no devuelve el resultado de esta llamada al método. La versión ficticia del método devuelve el resultado.

En el `Assert` paso, se asignan los`actualMessages`mensajes reales () de `Messages` la propiedad del modelo de página. También se realiza una comprobación de tipo cuando se asignan los mensajes. Los mensajes esperados y reales se comparan por sus `Text` propiedades. La prueba valida que las dos `List<Message>` instancias contienen los mismos mensajes.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Otras pruebas de este grupo crean objetos de modelo de página que <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>incluyen <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, <xref:Microsoft.AspNetCore.Mvc.ActionContext> , y `PageContext`para establecer `PageContext`, `ViewDataDictionary`y. Son útiles para realizar pruebas. Por ejemplo, la aplicación de mensaje establece `ModelState` un error <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> con para comprobar que se <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> devuelve un válido `OnPostAddMessageAsync` cuando se ejecuta:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Recursos adicionales

* [Pruebas C# unitarias en .net Core con pruebas de dotnet y xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Prueba unitaria del código](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Creación de una solución completa de .NET Core en macOS con Visual Studio para Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Introducción a xUnit.net: Uso de .NET Core con la línea de comandos del SDK de .NET](https://xunit.github.io/docs/getting-started-dotnet-core)
* [MOQ](https://github.com/moq/moq4)
* [Inicio rápido de MOQ](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core admite pruebas unitarias de aplicaciones de Razor Pages. Las pruebas de la capa de acceso a datos (DAL) y los modelos de página ayudan a garantizar:

* Las partes de una aplicación Razor Pages funcionan de forma independiente y conjunta como una unidad durante la construcción de la aplicación.
* Las clases y los métodos tienen ámbitos limitados de responsabilidad.
* Existe documentación adicional sobre el comportamiento de la aplicación.
* Las regresiones, que son errores que se producen al actualizar el código, se encuentran durante la creación e implementación automatizadas.

En este tema se supone que tiene conocimientos básicos de Razor Pages aplicaciones y pruebas unitarias. Si no está familiarizado con las aplicaciones de Razor Pages o los conceptos de prueba, vea los temas siguientes:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Pruebas C# unitarias en .net Core con pruebas de dotnet y xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

El proyecto de ejemplo se compone de dos aplicaciones:

| Aplicación         | Carpeta de proyecto                     | DESCRIPCIÓN |
| ----------- | ---------------------------------- | ----------- |
| Aplicación de mensaje | *src/RazorPagesTestSample*         | Permite que un usuario agregue un mensaje, elimine un mensaje, elimine todos los mensajes y analice los mensajes (busque el número promedio de palabras por mensaje). |
| Aplicación de prueba    | *tests/RazorPagesTestSample.Tests* | Se usa para probar unitarias el modelo de página de índice y la capa DAL de la aplicación de mensaje. |

Las pruebas se pueden ejecutar con las características de prueba integradas de un IDE, como [Visual Studio](/visualstudio/test/unit-test-your-code) o [Visual Studio para Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). Si usa [Visual Studio Code](https://code.visualstudio.com/) o la línea de comandos, ejecute el siguiente comando en un símbolo del sistema en la carpeta *tests/RazorPagesTestSample. tests* :

```console
dotnet test
```

## <a name="message-app-organization"></a>Organización de aplicaciones de mensajes

La aplicación de mensaje es un sistema de mensajes Razor Pages con las siguientes características:

* La página de índice de la aplicación (*pages/index. cshtml* y *pages/index. cshtml. CS*) proporciona un método de interfaz de usuario y un modelo de página para controlar la adición, eliminación y análisis de mensajes (buscar el número promedio de palabras por mensaje).
* La `Message` clase (*Data/Message. CS*) describe un mensaje con dos propiedades: `Id` (Key) y `Text` (Message). La `Text` propiedad es necesaria y está limitada a 200 caracteres.
* Los mensajes se almacenan mediante&#8224; [Entity Framework base de datos en memoria](/ef/core/providers/in-memory/).
* La aplicación contiene una capa Dal en su clase de contexto `AppDbContext` de base de datos, (*Data/AppDbContext. CS*). Los métodos Dal están marcados `virtual`, lo que permite simular los métodos para su uso en las pruebas.
* Si la base de datos está vacía al iniciarse la aplicación, el almacén de mensajes se inicializa con tres mensajes. Estos *mensajes inicializados* también se usan en las pruebas.

&#8224;En el tema EF, [Test with inmemory](/ef/core/miscellaneous/testing/in-memory), se explica cómo usar una base de datos en memoria para las pruebas con MSTest. En este tema se usa el marco de pruebas de [xUnit](https://xunit.github.io/) . Los conceptos de prueba y las implementaciones de prueba en diferentes marcos de pruebas son similares, pero no idénticos.

Aunque la aplicación de ejemplo no usa el patrón de repositorio y no es un ejemplo eficaz del [patrón de unidad de trabajo (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor pages admite estos patrones de desarrollo. Para obtener más información, vea [diseñar el nivel de persistencia de la infraestructura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) y <xref:mvc/controllers/testing> (el ejemplo implementa el patrón del repositorio).

## <a name="test-app-organization"></a>Probar la organización de la aplicación

La aplicación de prueba es una aplicación de consola dentro de la carpeta *tests/RazorPagesTestSample. tests* .

| Carpeta de aplicación de prueba | DESCRIPCIÓN |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.CS* contiene las pruebas unitarias para la capa dal.</li><li>*IndexPageTests.CS* contiene las pruebas unitarias para el modelo de página de índice.</li></ul> |
| *Utilidad*     | Contiene el `TestDbContextOptions` método utilizado para crear nuevas opciones de contexto de base de datos para cada prueba unitaria de la capa Dal, de modo que la base de datos se restablezca a su condición de línea base para cada prueba. |

El marco de pruebas es [xUnit](https://xunit.github.io/). El marco de trabajo ficticio de objetos es [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Pruebas unitarias de la capa de acceso a datos (DAL)

La aplicación de mensaje tiene un Dal con cuatro métodos contenidos en `AppDbContext` la clase (*src/RazorPagesTestSample/Data/AppDbContext. CS*). Cada método tiene una o dos pruebas unitarias en la aplicación de prueba.

| DAL (método)               | Función                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Obtiene de la base de datos ordenada por la `Text` propiedad. `List<Message>` |
| `AddMessageAsync`        | Agrega un `Message` a la base de datos.                                          |
| `DeleteAllMessagesAsync` | Elimina todas `Message` las entradas de la base de datos.                           |
| `DeleteMessageAsync`     | Elimina un único `Message` de la base de datos `Id`por.                      |

Las pruebas unitarias de la <xref:Microsoft.EntityFrameworkCore.DbContextOptions> capa Dal requieren al `AppDbContext` crear un nuevo para cada prueba. Un enfoque para crear el `DbContextOptions` para cada prueba es usar un: <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

El problema de este enfoque es que cada prueba recibe la base de datos en el estado que la prueba anterior la dejó. Esto puede ser problemático al intentar escribir pruebas unitarias atómicas que no interfieren entre sí. Para forzar `AppDbContext` a que use un nuevo contexto de base de datos para cada `DbContextOptions` prueba, proporcione una instancia de basada en un nuevo proveedor de servicios. La aplicación de prueba muestra cómo hacerlo mediante su `Utilities` método `TestDbContextOptions` de clase (*tests/RazorPagesTestSample. tests/Utilities/Utilities. CS*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

El `DbContextOptions` uso de en las pruebas unitarias de dal permite que cada prueba se ejecute de forma atómica con una nueva instancia de base de datos:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Cada método de prueba de `DataAccessLayerTest` la clase (*UnitTests/DataAccessLayerTest. CS*) sigue un patrón de Arrange-Act-Assert similar:

1. Orden La base de datos está configurada para la prueba y/o se ha definido el resultado esperado.
1. LEDs La prueba se ejecuta.
1. Declarar Se realizan aserciones para determinar si el resultado de la prueba es un éxito.

Por ejemplo, el `DeleteMessageAsync` método es responsable de quitar un solo mensaje identificado por su `Id` (*src/RazorPagesTestSample/Data/AppDbContext. CS*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Hay dos pruebas para este método. Una prueba comprueba que el método elimina un mensaje cuando el mensaje está presente en la base de datos. El otro método comprueba que la base de datos no cambia si `Id` no existe el mensaje para su eliminación. El `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` método se muestra a continuación:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

En primer lugar, el método realiza el paso de organización, donde tiene lugar la preparación para el paso de Act. Los mensajes de propagación se obtienen y se mantienen `seedMessages`en. Los mensajes de propagación se guardan en la base de datos. El mensaje con un `Id` de `1` se establece para su eliminación. Cuando se `DeleteMessageAsync` ejecuta el método, los mensajes esperados deben tener todos los mensajes excepto el que tiene un `Id` de `1`. La `expectedMessages` variable representa este resultado esperado.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

El método actúa: El `DeleteMessageAsync` método se ejecuta pasando el `recId` de `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Por último, el método obtiene el `Messages` del contexto y lo compara con la `expectedMessages` aserción de que los dos son iguales:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Para comparar que los dos `List<Message>` son los mismos:

* Los mensajes se ordenan `Id`por.
* Los pares de mensajes se comparan en la `Text` propiedad.

Un método `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` de prueba similar comprueba el resultado de intentar eliminar un mensaje que no existe. En este caso, los mensajes esperados en la base de datos deben ser iguales a los mensajes `DeleteMessageAsync` reales después de ejecutar el método. No debe haber ningún cambio en el contenido de la base de datos:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Pruebas unitarias de los métodos del modelo de página

Otro conjunto de pruebas unitarias es responsable de las pruebas de los métodos del modelo de página. En la aplicación de mensaje, los modelos de página de índice se `IndexModel` encuentran en la clase en *src/RazorPagesTestSample/pages/index. cshtml. CS*.

| Método del modelo de página | Función |
| ----------------- | -------- |
| `OnGetAsync` | Obtiene los mensajes de la capa Dal para la interfaz de usuario `GetMessagesAsync` mediante el método. |
| `OnPostAddMessageAsync` | Si el [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) es válido, llama `AddMessageAsync` a para agregar un mensaje a la base de datos. |
| `OnPostDeleteAllMessagesAsync` | Llama `DeleteAllMessagesAsync` a para eliminar todos los mensajes de la base de datos. |
| `OnPostDeleteMessageAsync` | Ejecuta para eliminar un mensaje con el `Id` especificado. `DeleteMessageAsync` |
| `OnPostAnalyzeMessagesAsync` | Si uno o más mensajes están en la base de datos, calcula el número promedio de palabras por mensaje. |

Los métodos del modelo de página se prueban con siete `IndexPageTests` pruebas en la clase (*tests/RazorPagesTestSample. tests/UnitTests/IndexPageTests. CS*). Las pruebas usan el conocido patrón Arrange-Assert-Act. Estas pruebas se centran en:

* Determinar si los métodos siguen el comportamiento correcto cuando [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) no es válido.
* La confirmación de los métodos produce el correcto <xref:Microsoft.AspNetCore.Mvc.IActionResult>.
* Comprobando que las asignaciones de valores de propiedad se realizan correctamente.

Este grupo de pruebas suele simular los métodos de la capa DAL para generar los datos esperados para el paso de la acción en el que se ejecuta un método de modelo de página. Por ejemplo, el `GetMessagesAsync` método `AppDbContext` de se simula para generar la salida. Cuando un método de modelo de página ejecuta este método, el simulacro devuelve el resultado. Los datos no provienen de la base de datos. Esto crea condiciones de prueba de confianza y predecible para usar la capa DAL en las pruebas del modelo de página.

La `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` prueba muestra cómo se `GetMessagesAsync` simula el método para el modelo de página:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Cuando el `OnGetAsync` método se ejecuta en el paso de Act, llama al método del modelo `GetMessagesAsync` de página.

Paso de acción de prueba unitaria (tests */RazorPagesTestSample. tests/UnitTests/IndexPageTests. CS*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`método del modelo `OnGetAsync` de página (*src/RazorPagesTestSample/pages/index. cshtml. CS*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

El `GetMessagesAsync` método de la capa Dal no devuelve el resultado de esta llamada al método. La versión ficticia del método devuelve el resultado.

En el `Assert` paso, se asignan los`actualMessages`mensajes reales () de `Messages` la propiedad del modelo de página. También se realiza una comprobación de tipo cuando se asignan los mensajes. Los mensajes esperados y reales se comparan por sus `Text` propiedades. La prueba valida que las dos `List<Message>` instancias contienen los mismos mensajes.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Otras pruebas de este grupo crean objetos de modelo de página que <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>incluyen <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, <xref:Microsoft.AspNetCore.Mvc.ActionContext> , y `PageContext`para establecer `PageContext`, `ViewDataDictionary`y. Son útiles para realizar pruebas. Por ejemplo, la aplicación de mensaje establece `ModelState` un error <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> con para comprobar que se <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> devuelve un válido `OnPostAddMessageAsync` cuando se ejecuta:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Recursos adicionales

* [Pruebas C# unitarias en .net Core con pruebas de dotnet y xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Prueba unitaria del código](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Creación de una solución completa de .NET Core en macOS con Visual Studio para Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Introducción a xUnit.net: Uso de .NET Core con la línea de comandos del SDK de .NET](https://xunit.github.io/docs/getting-started-dotnet-core)
* [MOQ](https://github.com/moq/moq4)
* [Inicio rápido de MOQ](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
