---
title: Pruebas de integración de ASP.NET Core
author: guardrex
description: Obtenga información acerca de cómo las pruebas de integración garantizan que los componentes de una aplicación funcionen correctamente en el nivel de infraestructura, incluida la base de datos, el sistema de archivos y la red.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/integration-tests
ms.openlocfilehash: a402c3f5f6a75917eba4058e6cc6926f25b214d4
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217744"
---
# <a name="integration-tests-in-aspnet-core"></a>Pruebas de integración de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex) y [Steve Smith](https://ardalis.com/)

Pruebas de integración aseguran de que los componentes de una aplicación funcionen correctamente en un nivel que incluye la infraestructura de soporte de la aplicación, como la base de datos, el sistema de archivos y la red. ASP.NET Core es compatible con las pruebas de integración con un marco de pruebas unitarias con un host de web de prueba y un servidor de prueba en memoria.

Este tema supone un conocimiento básico de las pruebas unitarias. Si familiarizado con conceptos de prueba, consulte el [pruebas unitarias en .NET Core y estándar de .NET](/dotnet/core/testing/) tema y su contenido vinculado.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

La aplicación de ejemplo es una aplicación de páginas de Razor y asume un conocimiento básico de las páginas de Razor. Si no conoce las páginas de Razor, vea los temas siguientes:

* [Introducción a las páginas de Razor](xref:mvc/razor-pages/index)
* [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Pruebas unitarias de páginas de Razor](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Introducción a las pruebas de integración

Pruebas de integración evaluación componentes de una aplicación en un nivel más amplio que [pruebas unitarias](/dotnet/core/testing/). Pruebas unitarias se usan para probar los componentes de software independiente, como métodos de clase individuales. Pruebas de integración confirmación que dos o más componentes de aplicación funcionan conjuntamente para generar un resultado esperado, posiblemente incluyen todos los componentes necesarios para procesar completamente una solicitud.

Estas pruebas más amplias se usan para probar la aplicación en la infraestructura y todo framework, a menudo incluye los siguientes componentes:

* Base de datos
* Sistema de archivos
* Dispositivos de red
* Canalización de solicitudes y respuestas

Pruebas unitarias uso fabricado componentes, denominados *fakes* o *simular objetos*, en lugar de los componentes de infraestructura.

A diferencia de pruebas unitarias, pruebas de integración:

* Utilice los componentes reales que usa la aplicación en producción.
* Se requieren más código y procesamiento de datos.
* Tardar más tiempo en ejecutarse.

Por lo tanto, limite el uso de pruebas de integración para los escenarios más importantes de la infraestructura. Si un comportamiento se puede probar con una prueba unitaria o una prueba de integración, elija la prueba unitaria.

> [!TIP]
> No escriba pruebas de integración para cada permutación posibles de acceso de archivos y datos con sistemas de archivos y bases de datos. Independientemente de cuántos lugares a través de una aplicación interactuar con bases de datos y sistemas de archivos, un conjunto de lectura, escritura, actualización y eliminación integración pruebas son normalmente capaces de base de datos de prueba adecuadamente y componentes del sistema de archivos tiene el foco. Usar unidad de pruebas para pruebas de rutina de la lógica del método que interactúan con estos componentes. En pruebas unitarias, el uso de la infraestructura de fakes/simulacros resultado en una ejecución de pruebas más rápida.

> [!NOTE]
> En las discusiones de pruebas de integración, el proyecto probado se suele denominar la *sistema sometido a prueba*, o "SUT" para abreviar.

## <a name="aspnet-core-integration-tests"></a>Pruebas de integración de ASP.NET Core

Pruebas de integración de ASP.NET Core requieren lo siguiente:

* Un proyecto de prueba se usa para contener y ejecutar las pruebas. El proyecto de prueba tiene una referencia al proyecto de ASP.NET Core probado, denominado el *sistema sometido a prueba* (SUT). _"SUT" se utiliza a lo largo de este tema para hacer referencia a la aplicación probada._
* El proyecto de prueba crea un host de web de prueba para el SUT y usa a un cliente de servidor de prueba para controlar las solicitudes y respuestas para el SUT.
* Se utiliza un ejecutor de pruebas para ejecutar las pruebas y los informes de los resultados de pruebas.

Pruebas de integración siguen una secuencia de eventos que incluyen el habitual *organizar*, *Act*, y *Assert* pasos de prueba:

1. Host de web de SUT está configurado.
1. Se crea un cliente de servidor de prueba para enviar solicitudes a la aplicación.
1. El *organizar* se ejecuta el paso de prueba: la aplicación de prueba prepara una solicitud.
1. El *Act* se ejecuta el paso de prueba: el cliente envía la solicitud y recibe la respuesta.
1. El *Assert* se ejecuta el paso de prueba: el *real* respuesta se valida como un *pasar* o *producirá un error* tomando como base un *esperado*  respuesta.
1. El proceso continúa hasta que todas las pruebas se ejecutan.
1. Se informa de los resultados de pruebas.

Normalmente, el host de prueba web está configurado de forma diferente de host de la aplicación web normal para la prueba se ejecuta. Por ejemplo, podría utilizarse para las pruebas de otra base de datos o la configuración de aplicación distinta.

Componentes de infraestructura, como el host de prueba web y el servidor de prueba en memoria ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), se proporciona o administrados por el [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paquete. Uso de este paquete simplifica la creación de la prueba y ejecución.

El `Microsoft.AspNetCore.Mvc.Testing` paquete controla las siguientes tareas:

* Copia el archivo de dependencias (*\*.deps*) desde el SUT en el proyecto de prueba *bin* carpeta.
* Establece la raíz de contenido en la raíz del proyecto de SUT para que se encuentran archivos estáticos como páginas o vistas cuando se ejecutan las pruebas.
* Proporciona el [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) clase simplificar arrancar el SUT con `TestServer`.

El [pruebas unitarias](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentación describe cómo configurar un proyecto y probar el ejecutor de pruebas, así como instrucciones detalladas sobre cómo ejecutar pruebas y recomendaciones sobre cómo para comprobaciones de nombres y clases de prueba.

> [!NOTE]
> Al crear un proyecto de prueba para una aplicación, separe las pruebas unitarias de las pruebas de integración en proyectos diferentes. Esto ayuda a asegurarse de que accidentalmente componentes de infraestructura de pruebas no incluidos en las pruebas unitarias. La separación de las pruebas de unidades y de integración también permite controlar en qué conjunto de pruebas se ejecutan.

No hay prácticamente ninguna diferencia entre la configuración de pruebas de las aplicaciones de las páginas de Razor y las aplicaciones MVC. La única diferencia está en cómo se denominan las pruebas. En una aplicación de páginas de Razor, suelen denominarse pruebas de los puntos de conexión de la página después de la clase de modelo de página (por ejemplo, `IndexPageTests` para probar la integración del componente de la página de índice). En una aplicación MVC, pruebas normalmente están organizadas por las clases de controlador y los controladores que se someten a prueba con el nombre (por ejemplo, `HomeControllerTests` para probar la integración del componente para el controlador Home).

## <a name="test-app-prerequisites"></a>Requisitos previos de la aplicación de prueba

El proyecto de prueba debe:

* Tiene una referencia de paquete para [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/).
* Use el SDK de Web en el archivo de proyecto (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Estos prerequesities puede verse en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Inspeccionar el *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* archivo.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Pruebas básicas con el valor predeterminado WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) se utiliza para crear un [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) para las pruebas de integración. `TEntryPoint` Normalmente es la clase de punto de entrada de la SUT la `Startup` clase.

Implementar clases de prueba un *accesorio de clase* interfaz (`IClassFixture`) para indicar la clase contiene pruebas y proporciona instancias de objetos compartidos a través de las pruebas de la clase.

### <a name="basic-test-of-app-endpoints"></a>Prueba básica de extremos de la aplicación

La siguiente prueba (clase), `BasicTests`, usa el `WebApplicationFactory` para arrancar el SUT y proporcionar un [HttpClient](/dotnet/api/system.net.http.httpclient) a un método de prueba, `Get_EndpointsReturnSuccessAndCorrectContentType`. El método comprueba si el código de estado de respuesta es correcto (códigos de estado en el intervalo de 200-299) y la `Content-Type` encabezado es `text/html; charset=utf-8` para varias páginas de la aplicación.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crea una instancia de `HttpClient` que sigue redirecciones y controla las cookies automáticamente.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Probar un extremo seguro

Otra prueba en el `BasicTests` clase comprueba que un punto de conexión seguro redirige un usuario no autenticado a la página de inicio de sesión de la aplicación.

En el SUT el `/SecurePage` página usa un [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convención para aplicar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página. Para obtener más información, consulte [convenciones de autorización de las páginas de Razor](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

En el `Get_SecurePageRequiresAnAuthenticatedUser` probar, un [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) está establecido en no permitir redirecciones estableciendo [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) a `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Al no permitir el cliente sigue la redirección, se pueden realizar las siguientes comprobaciones:

* Se puede comprobar el código de estado devuelto por el SUT con el esperado [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) resultado, no el código de estado final después de la redirección a la página de inicio de sesión, que sería [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* El `Location` se comprueba el valor del encabezado de los encabezados de respuesta para confirmar que inicia con `http://localhost/Identity/Account/Login`, no el inicio de sesión página respuesta final, donde el `Location` encabezado no estar presente.

Para obtener más información sobre `WebApplicationFactoryClientOptions`, consulte el [opciones de cliente](#client-options) sección.

## <a name="customize-webapplicationfactory"></a>Personalizar WebApplicationFactory

Configuración del host Web puede crearse independientemente de las clases de prueba mediante la adquisición de `WebApplicationFactory` para crear uno o varios generadores personalizados:

1. Heredar de `WebApplicationFactory` e invalide [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). El [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) permite la configuración de la colección de servicio con [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   En la propagación de la base de datos la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) realiza el `InitializeDbForTests` método. El método se describe en el [ejemplo de pruebas de integración: organización de la aplicación de prueba](#test-app-organization) sección.

2. Utilice la opción de instalación `CustomWebApplicationFactory` en las clases de prueba. En el ejemplo siguiente se utiliza el generador en el `IndexPageTests` clase:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Cliente de la aplicación de ejemplo está configurado para impedir la `HttpClient` de redirecciones siguientes. Como se explica en la [probar un extremo seguro](#test-a-secure-endpoint) sección, esto permite a pruebas para comprobar el resultado de la primera respuesta de la aplicación. La primera respuesta es un redireccionamiento en muchas de estas pruebas con un `Location` encabezado.

3. Utiliza una prueba normal el `HttpClient` y métodos auxiliares para procesar la solicitud y la respuesta:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Cualquier solicitud POST a la SUT debe cumplir la comprobación de antiforgery que se convierte automáticamente en la aplicación [antiforgery sistema de protección de datos](xref:security/data-protection/introduction). Para organizar de solicitud POST de una prueba, la aplicación de prueba debe:

1. Realice una solicitud de la página.
1. Analizar la cookie antiforgery y el token de validación de solicitud de la respuesta.
1. Asegúrese de la solicitud POST con la validación de solicitud y cookie antiforgery símbolo (token) en su lugar.

El `SendAsync` métodos de extensión de aplicación auxiliar (*Helpers/HttpClientExtensions.cs*) y la `GetDocumentAsync` método auxiliar (*Helpers/HtmlHelpers.cs*) en el [deaplicacióndeejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) usar la [AngleSharp](https://anglesharp.github.io/) analizador para controlar la comprobación de antiforgery con los métodos siguientes:

* `GetDocumentAsync` &ndash; Recibe la [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) y devuelve un `IHtmlDocument`. `GetDocumentAsync` usa un generador que prepara un *respuesta virtual* basado en el original `HttpResponseMessage`. Para obtener más información, consulte el [AngleSharp documentación](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` métodos de extensión para la `HttpClient` redactar una [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) y llame a [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) para enviar solicitudes a la SUT. Sobrecargas de `SendAsync` acepte el formulario HTML (`IHtmlFormElement`) y los siguientes:
  - Botón del formulario de envío (`IHtmlElement`)
  - Colección de valores de formulario (`IEnumerable<KeyValuePair<string, string>>`)
  - Botón de envío (`IHtmlElement`) y valores del formulario (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) se utiliza con fines de demostración, en este tema y la aplicación de ejemplo de biblioteca de análisis de un tercero. AngleSharp no es compatible o requerida para las pruebas de integración de aplicaciones de ASP.NET Core. Pueden utilizar otros analizadores, como el [Html agilidad Pack (GRACIA)](http://html-agility-pack.net/). Otro enfoque es escribir código para controlar el sistema antiforgery token de comprobación de solicitud y cookie antiforgery directamente.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personalizar al cliente con WithWebHostBuilder

Cuando se requiere un método de prueba, configuración adicional [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crea un nuevo `WebApplicationFactory` con una [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) que más se personaliza mediante configuración.

El `Post_DeleteMessageHandler_ReturnsRedirectToRoot` probar el método de la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) muestra el uso de `WithWebHostBuilder`. Esta prueba realiza una eliminación de registro en la base de datos mediante la activación de un envío de formulario en el SUT.

Porque otra prueba en el `IndexPageTests` clase realiza una operación que eliminará todos los registros en la base de datos y se puede ejecutar antes de la `Post_DeleteMessageHandler_ReturnsRedirectToRoot` (método), la base de datos es visible en este método de prueba para asegurarse de que está presente para el SUT eliminar un registro. Al seleccionar la `deleteBtn1` botón de la `messages` se simula el formulario en el SUT en la solicitud a la SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opciones de cliente

En la tabla siguiente se muestra el valor predeterminado [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponibles al crear `HttpClient` instancias.

| Opción | Descripción | Default |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Obtiene o establece si `HttpClient` instancias deben seguir automáticamente las respuestas de redirección. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Obtiene o establece la dirección base del `HttpClient` instancias. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Obtiene o establece si `HttpClient` instancias deben controlar las cookies. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Obtiene o establece el número máximo de respuestas de redirección que `HttpClient` las instancias deben seguir. | 7 |

Crear el `WebApplicationFactoryClientOptions` clase y lo pasa a la [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (método) (valor predeterminado en el ejemplo de código se muestran los valores):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Cómo la infraestructura de prueba deduce la ruta de acceso de contenido raíz de aplicación

El `WebApplicationFactory` constructor deduce la ruta de acceso de contenido raíz de aplicación mediante la búsqueda de un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) en el ensamblado que contiene las pruebas de integración con una clave igual a la `TEntryPoint` ensamblado `System.Reflection.Assembly.FullName`. En caso de que no se encuentra un atributo con la clave correcta, `WebApplicationFactory` vuelve a buscar un archivo de solución (*\*.sln*) y anexa el `TEntryPoint` nombre del ensamblado para el directorio de la solución. El directorio raíz de aplicación (la ruta de acceso raíz del contenido) se usa para detectar las vistas y los archivos de contenido.

En la mayoría de los casos, no será necesario establecer explícitamente la raíz de contenido de la aplicación, como la lógica de búsqueda normalmente busca la raíz de contenido correcta en tiempo de ejecución. En casos especiales que no se encuentra la raíz de contenido mediante el algoritmo de búsqueda integradas, la aplicación de contenido raíz se puede especificar explícitamente o mediante el uso de una lógica personalizada. Para establecer la raíz de contenido de aplicación en esos escenarios, llame a la `UseSolutionRelativeContentRoot` método de extensión de la [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) paquete. Proporcione la ruta de acceso relativa de la solución y el patrón de nombre o glob del archivo de solución opcional (valor predeterminado = `*.sln`).

Llame a la [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) método de extensión mediante *una* de los métodos siguientes:

* Al configurar las clases de prueba con `WebApplicationFactory`, proporcione una configuración personalizada con el [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* Al configurar las clases de prueba con un personalizado `WebApplicationFactory`, heredar de `WebApplicationFactory` e invalide [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>Deshabilitar las copias sombra

Instantáneas hace que las pruebas se ejecutan en una carpeta diferente a la carpeta de salida. Para que las pruebas para que funcione correctamente, debe deshabilitarse la operación de instantánea. El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) utiliza xUnit y deshabilita la operación de copia sombra para xUnit mediante la inclusión de un *xunit.runner.json* archivo con la opción de configuración correcto. Para obtener más información, consulte [configurar xUnit.net con JSON](https://xunit.github.io/docs/configuring-with-json.html).

Agregar el *xunit.runner.json* archivo al nodo raíz del proyecto de prueba con el siguiente contenido:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Ejemplo de pruebas de integración

El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) se compone de dos aplicaciones:

| Aplicación | Carpeta de proyecto | Descripción |
| --- | -------------- | ----------- |
| Aplicación de mensaje (el SUT) | *src/RazorPagesProject* | Permite a un usuario agregar, eliminar uno, elimine todos y analizar los mensajes. |
| Aplicación de prueba | *tests/RazorPagesProject.Tests* | Utilizado para la prueba de integración de la SUT. |

Se pueden ejecutar las pruebas con las características integradas de prueba de un IDE, como [Visual Studio](https://www.visualstudio.com/vs/). Si usa [código de Visual Studio](https://code.visualstudio.com/) o la línea de comandos, ejecute el siguiente comando en un símbolo del sistema en el *tests/RazorPagesProject.Tests* carpeta:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organización de la aplicación (SUT) de mensaje

El SUT es un sistema de mensajes de las páginas de Razor con las siguientes características:

* La página de índice de la aplicación (*Pages/Index.cshtml* y *Pages/Index.cshtml.cs*) proporciona una interfaz de usuario y la página métodos del modelo para controlar la adición, eliminación y análisis de mensajes (palabras medios por mensaje) .
* Describe un mensaje de la `Message` clase (*Data/Message.cs*) con dos propiedades: `Id` (clave) y `Text` (mensaje). El `Text` propiedad es necesaria y se limita a 200 caracteres.
* Los mensajes se almacenan con [base de datos de Entity Framework en memoria](/ef/core/providers/in-memory/)&#8224;.
* La aplicación contiene una capa de acceso a datos (DAL) en su clase de contexto de base de datos, `AppDbContext` (*Data/AppDbContext.cs*).
* Si la base de datos está vacía en el inicio de la aplicación, el almacén de mensajes se inicializa con tres mensajes.
* La aplicación incluye un `/SecurePage` que sólo pueda tener acceso un usuario autenticado.

&#8224;El tema EF [prueba con InMemory](/ef/core/miscellaneous/testing/in-memory), explica cómo utilizar una base de datos en memoria para las pruebas con MSTest. Este tema se usa el [xUnit](https://xunit.github.io/) marco de pruebas. Las implementaciones de prueba a través de los marcos de pruebas diferente y conceptos de prueba son similares pero no idénticos.

Aunque la aplicación no usa el [modelo de repositorio](http://martinfowler.com/eaaCatalog/repository.html) y no es un ejemplo efectivo de la [patrón de la unidad de trabajo (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), las páginas de Razor admite estos patrones de desarrollo. Para obtener más información, consulte [diseñar la capa de persistencia de infraestructura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementación del repositorio y patrones de unidad de trabajo en una aplicación de MVC de ASP.NET](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), y [controlador de pruebas lógica de](/aspnet/core/mvc/controllers/testing) (el ejemplo implementa el modelo de repositorio).

### <a name="test-app-organization"></a>Organización de la aplicación de prueba

La aplicación de prueba es una aplicación de consola en el *tests/RazorPagesProject.Tests* carpeta.

| Carpeta de la aplicación de prueba | Descripción |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* contiene métodos de prueba para el enrutamiento, obtener acceso a una página segura por un usuario no autenticado y obtener un perfil de usuario de GitHub y comprobación de inicio de sesión de usuario del perfil. |
| *IntegrationTests* | *IndexPageTests.cs* contiene las pruebas de integración de la página de índice mediante personalizado `WebApplicationFactory` clase. |
| *Aplicaciones auxiliares/utilidades* | <ul><li>*Utilities.cs* contiene el `InitializeDbForTests` método utilizado para inicializar la base de datos con datos de prueba.</li><li>*HtmlHelpers.cs* proporciona un método para devolver un AngleSharp `IHtmlDocument` para su uso por los métodos de prueba.</li><li>*HttpClientExtensions.cs* proporcionan sobrecargas para `SendAsync` para enviar solicitudes a la SUT.</li></ul> |

El marco de pruebas es [xUnit](https://xunit.github.io/). Pruebas de integración se realizan empleando el [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), que incluye el [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Dado que la [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paquete se utiliza para configurar el servidor de prueba y el host de prueba, el `TestHost` y `TestServer` paquetes no requieren referencias del paquete directa en el archivo de proyecto de la aplicación de prueba o configuración de desarrollador de la aplicación de prueba.

**La propagación de la base de datos para las pruebas**

Pruebas de integración suelen requieran un pequeño conjunto de datos en la base de datos antes de la ejecución de prueba. Por ejemplo, una eliminación pruebas llamadas en busca de una eliminación de registros de base de datos, de modo que la base de datos debe tener al menos un registro para la solicitud de eliminación tenga éxito.

Valores de inicialización de la aplicación de ejemplo con tres mensajes en la base de datos *Utilities.cs* que las pruebas se pueden usar cuando ejecuta:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Recursos adicionales

* [Pruebas unitarias](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Pruebas unitarias de páginas de Razor](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Controladores de pruebas](xref:mvc/controllers/testing)
