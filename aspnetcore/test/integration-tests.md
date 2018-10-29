---
title: Pruebas de integración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo las pruebas de integración garantizan que los componentes de una aplicación, como la base de datos, el sistema de archivos y la red, funcionen correctamente en la infraestructura.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: a136a362cd8973b3684f9a70bd4792d75238eab0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207880"
---
# <a name="integration-tests-in-aspnet-core"></a>Pruebas de integración en ASP.NET Core

Por [Halter](https://github.com/guardrex) y [Steve Smith](https://ardalis.com/)

Las pruebas de integración aseguran de que los componentes de una aplicación funcionen correctamente en un nivel que incluye la infraestructura de soporte de la aplicación, como la base de datos, el sistema de archivos y la red. ASP.NET Core es compatible con las pruebas de integración con un marco de pruebas unitarias con un host de prueba web y un servidor de prueba en memoria.

En este tema se da por supuesto un conocimiento básico de las pruebas unitarias. Si no conoce los conceptos de pruebas, consulte el [Unit Testing en .NET Core y .NET Standard](/dotnet/core/testing/) tema y su contenido vinculado.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

La aplicación de ejemplo es una aplicación de páginas de Razor y se da por supuesto un conocimiento básico de las páginas de Razor. Si no conoce las páginas de Razor, vea los temas siguientes:

* [Introducción a las páginas de Razor](xref:razor-pages/index)
* [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Pruebas unitarias de páginas de Razor](xref:test/razor-pages-tests)

> [!NOTE]
> Para probar las SPA, se recomienda una herramienta como [Selenium](https://www.seleniumhq.org/), que puede automatizar un explorador.

## <a name="introduction-to-integration-tests"></a>Introducción a las pruebas de integración

Las pruebas de integración para evaluar los componentes de una aplicación en un nivel más amplio que [pruebas unitarias](/dotnet/core/testing/). Las pruebas unitarias se utilizan para probar los componentes de software independiente, como los métodos de la clase individual. Confirmación que las pruebas de integración que dos o más componentes de aplicación funcionan conjuntamente para producir un resultado esperado, posiblemente incluidos todos los componentes necesarios para procesar una solicitud.

Estas pruebas más amplias se utilizan para probar la infraestructura de la aplicación y un marco completo, a menudo incluye los siguientes componentes:

* Base de datos
* Sistema de archivos
* Dispositivos de red
* Canalización de solicitud y respuesta

Pruebas unitarias use fabricado componentes, denominados *fakes* o *objetos ficticios*, en lugar de los componentes de infraestructura.

A diferencia de las pruebas unitarias, pruebas de integración:

* Utilice los componentes reales que usa la aplicación en producción.
* Se requieren más código y procesamiento de datos.
* Tardan más en ejecutarse.

Por lo tanto, limite el uso de las pruebas de integración para los escenarios de infraestructura más importantes. Si un comportamiento se puede probar con una prueba unitaria o una prueba de integración, elija la prueba unitaria.

> [!TIP]
> No escriba las pruebas de integración para todas las posibles permutaciones de acceso de archivos y datos con las bases de datos y sistemas de archivos. Independientemente de cuántos lugares a través de una aplicación interactuar con las bases de datos y sistemas de archivos, un conjunto con foco de lectura, escritura, actualización y eliminación integración pruebas son normalmente capaces de adecuadamente las pruebas de la base de datos y los componentes del sistema de archivos. Use pruebas unitarias para las pruebas de rutina de la lógica del método que interactúan estos componentes. En las pruebas unitarias, el uso de la infraestructura de fakes/simulacros resultado en ejecución de pruebas más rápida.

> [!NOTE]
> En las conversaciones de pruebas de integración, se suele denominar el proyecto probado el *sistema sometido a prueba*, o "SUT" para abreviar.

## <a name="aspnet-core-integration-tests"></a>Pruebas de integración de ASP.NET Core

Las pruebas de integración en ASP.NET Core requieren lo siguiente:

* Un proyecto de prueba se usa para contener y ejecutar las pruebas. El proyecto de prueba tiene una referencia al proyecto de ASP.NET Core probado, denominado el *sistema sometido a prueba* (SUT). _"SUT" se utiliza a lo largo de este tema para hacer referencia a la aplicación probada._
* El proyecto de prueba crea un host de web de prueba para el SUT y usa a un cliente del servidor de prueba para controlar las solicitudes y respuestas al SUT.
* Un ejecutor de pruebas se usa para ejecutar las pruebas y el informe los resultados de pruebas.

Las pruebas de integración, siga una secuencia de eventos que incluyen el habitual *organizar*, *Act*, y *Assert* pasos de prueba:

1. Se configura el host de web del SUT.
1. Se crea un cliente de servidor de prueba para enviar solicitudes a la aplicación.
1. El *organizar* se ejecuta el paso de prueba: la aplicación de prueba prepara una solicitud.
1. El *Act* se ejecuta el paso de prueba: el cliente envía la solicitud y recibe la respuesta.
1. El *Assert* se ejecuta el paso de prueba: el *real* respuesta se valida como un *pasar* o *producirá un error en* según un *esperado*  respuesta.
1. El proceso continúa hasta que todas las pruebas se ejecutan.
1. Se notifican los resultados de pruebas.

Por lo general, el host de prueba web está configurado de forma diferente de host de la aplicación web normal para la prueba se ejecuta. Por ejemplo, podría utilizarse para las pruebas de otra base de datos o la configuración de aplicación diferentes.

Componentes de infraestructura, como el host de prueba web y el servidor de prueba en memoria ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), se proporciona o administrados por el [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paquete. Uso de este paquete simplifica la creación de pruebas y la ejecución.

El `Microsoft.AspNetCore.Mvc.Testing` paquete controla las tareas siguientes:

* Copia el archivo de dependencias (*\*.deps*) desde el SUT en el proyecto de prueba *bin* carpeta.
* Establece la raíz del contenido en la raíz del proyecto del SUT para que se encuentran los archivos estáticos y páginas o vistas cuando se ejecutan las pruebas.
* Proporciona el [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) clase para simplificar el arranque del SUT con `TestServer`.

El [pruebas unitarias](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentación describe cómo configurar un proyecto y prueba el ejecutor de pruebas, junto con instrucciones detalladas sobre cómo ejecutar pruebas y recomendaciones sobre cómo para comprobaciones de nombres y clases de prueba.

> [!NOTE]
> Al crear un proyecto de prueba para una aplicación, separe las pruebas unitarias de las pruebas de integración en proyectos diferentes. Esto ayuda a asegurarse de que accidentalmente componentes de infraestructura de pruebas no incluidas en las pruebas unitarias. Separación de las pruebas unitarias y de integración también permite controlar qué conjunto de pruebas se ejecutan.

No hay prácticamente ninguna diferencia entre la configuración de pruebas de aplicaciones de las páginas de Razor y las aplicaciones MVC. La única diferencia está en cómo se denominan las pruebas. En una aplicación de páginas de Razor, suelen denominarse pruebas de puntos de conexión de página después de la clase de modelo de página (por ejemplo, `IndexPageTests` para probar la integración del componente de la página de índice). En una aplicación MVC, las pruebas normalmente se organizada por las clases de controlador y los controladores que se prueban con el nombre (por ejemplo, `HomeControllerTests` para probar la integración del componente para el controlador Home).

## <a name="test-app-prerequisites"></a>Requisitos previos de la aplicación de prueba

El proyecto de prueba debe:

* Hacer referencia a los siguientes paquetes:
  - [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  - [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Especifique el SDK de Web en el archivo de proyecto (`<Project Sdk="Microsoft.NET.Sdk.Web">`). El SDK de Web es necesario cuando se hace referencia el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).

Estos requisitos previos se pueden ver en el [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Inspeccionar el *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* archivo. La aplicación de ejemplo usa el [xUnit](https://xunit.github.io/) marco de pruebas y la [AngleSharp](https://anglesharp.github.io/) biblioteca analizador, por lo que también hace referencia a la aplicación de ejemplo:

* [xUnit](https://www.nuget.org/packages/xunit/)
* [xUnit.Runner.VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Pruebas básicas con el valor predeterminado WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) se utiliza para crear un [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) para las pruebas de integración. `TEntryPoint` es la clase de punto de entrada del SUT, normalmente la `Startup` clase.

Clases de prueba implementan un *accesorio clase* interfaz (`IClassFixture`) para indicar la clase contiene las pruebas y proporciona instancias de objetos compartidos entre las pruebas de la clase.

### <a name="basic-test-of-app-endpoints"></a>Prueba básica de los puntos de conexión de la aplicación

La siguiente clase, de prueba `BasicTests`, usa el `WebApplicationFactory` para arrancar el SUT y proporcionar un [HttpClient](/dotnet/api/system.net.http.httpclient) a un método de prueba, `Get_EndpointsReturnSuccessAndCorrectContentType`. El método comprueba si el código de estado de respuesta es correcto (códigos de estado en el intervalo 200-299) y el `Content-Type` encabezado es `text/html; charset=utf-8` de varias páginas de aplicación.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crea una instancia de `HttpClient` que sigue las redirecciones y controla las cookies automáticamente.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Probar un extremo seguro

Otra prueba en el `BasicTests` clase comprueba que un extremo seguro redirige a un usuario no autenticado a la página de inicio de sesión de la aplicación.

En el SUT, el `/SecurePage` página usa un [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convención para aplicar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página. Para obtener más información, consulte [convenciones de autorización de las páginas de Razor](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

En el `Get_SecurePageRequiresAnAuthenticatedUser` probar, un [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) está establecido en no permitir redireccionamientos estableciendo [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) a `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Al no permitir el cliente sigue la redirección, se pueden realizar las siguientes comprobaciones:

* El código de estado devuelto por el SUT puede comprobarse con el esperado [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) resultado, no el código de estado final después de la redirección a la página de inicio de sesión, que sería [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* El `Location` se comprueba el valor del encabezado de los encabezados de respuesta para confirmar que se inicia con `http://localhost/Identity/Account/Login`, no el inicio de sesión página respuesta final, donde el `Location` encabezado no estar presente.

Para obtener más información sobre `WebApplicationFactoryClientOptions`, consulte el [opciones de cliente](#client-options) sección.

## <a name="customize-webapplicationfactory"></a>Personalizar WebApplicationFactory

Configuración del host Web puede crearse independientemente de las clases de prueba mediante la herencia de `WebApplicationFactory` para crear uno o varios de los generadores personalizados:

1. Heredar de `WebApplicationFactory` e invalidar [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). El [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) permite la configuración de la colección de servicios con [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   En la propagación de la base de datos la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) se realiza mediante el `InitializeDbForTests` método. El método se describe en el [ejemplo de pruebas de integración: organización de la aplicación de prueba](#test-app-organization) sección.

2. Usar personalizado `CustomWebApplicationFactory` en las clases de prueba. En el ejemplo siguiente se usa el generador en el `IndexPageTests` clase:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Cliente de la aplicación de ejemplo está configurada para evitar el `HttpClient` de redireccionamientos siguientes. Como se explica en el [probar un extremo seguro](#test-a-secure-endpoint) sección, esto permite que las pruebas para comprobar el resultado de la primera respuesta de la aplicación. La primera respuesta es un redireccionamiento en muchas de estas pruebas con un `Location` encabezado.

3. Una prueba normal se usa el `HttpClient` y métodos auxiliares para procesar la solicitud y la respuesta:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Cualquier solicitud POST al SUT debe cumplir la comprobación de antifalsificación se convierte automáticamente en la aplicación [sistema antifalsificación de protección de datos](xref:security/data-protection/introduction). Para organizar de la solicitud POST de una prueba, la prueba de la aplicación debe:

1. Realice una solicitud para la página.
1. Analizar la cookie antifalsificación y el token de solicitud de validación de la respuesta.
1. Realizar la solicitud POST con la validación de solicitud y la cookie antifalsificación token en su lugar.

El `SendAsync` métodos de extensión de aplicación auxiliar (*Helpers/HttpClientExtensions.cs*) y el `GetDocumentAsync` método auxiliar (*Helpers/HtmlHelpers.cs*) en el [deaplicacióndeejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) utilizar el [AngleSharp](https://anglesharp.github.io/) analizador para controlar la comprobación de antifalsificación con los métodos siguientes:

* `GetDocumentAsync` &ndash; Recibe el [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) y devuelve un `IHtmlDocument`. `GetDocumentAsync` utiliza un generador que prepara un *respuesta virtual* basado en el original `HttpResponseMessage`. Para obtener más información, consulte el [AngleSharp documentación](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` métodos de extensión para el `HttpClient` redactar una [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) y llamar a [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) para enviar solicitudes al SUT. Sobrecargas para `SendAsync` acepte el formulario HTML (`IHtmlFormElement`) y lo siguiente:
  - Botón del formulario de envío (`IHtmlElement`)
  - Colección de valores de formulario (`IEnumerable<KeyValuePair<string, string>>`)
  - Botón de envío (`IHtmlElement`) y valores de formulario (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) es biblioteca que se usa para fines de demostración en este tema y la aplicación de ejemplo de análisis de un tercero. AngleSharp no es compatible o necesarias para las pruebas de integración de aplicaciones de ASP.NET Core. Se pueden usar otros analizadores, como el [Html agilidad Pack (GRACIA)](http://html-agility-pack.net/). Otro enfoque consiste en escribir código para controlar el token de comprobación de solicitud y la cookie antifalsificación el sistema antifalsificación directamente.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personalización del cliente con WithWebHostBuilder

Cuando se necesita dentro de un método de prueba, configuración adicional [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crea un nuevo `WebApplicationFactory` con un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) que se personaliza más aún mediante configuración.

El `Post_DeleteMessageHandler_ReturnsRedirectToRoot` probar el método de la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) muestra el uso de `WithWebHostBuilder`. Esta prueba realiza una eliminación de registro en la base de datos mediante la activación de un envío de formulario en el SUT.

Porque otra prueba en el `IndexPageTests` clase realiza una operación que elimina todos los registros en la base de datos y puede ejecutar antes la `Post_DeleteMessageHandler_ReturnsRedirectToRoot` método, la base de datos es visible en este método de prueba para asegurarse de que está presente para el SUT eliminar un registro. Seleccionar el `deleteBtn1` botón de la `messages` se simula el formulario en el SUT en la solicitud al SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opciones de cliente

En la tabla siguiente se muestra el valor predeterminado [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponibles al crear `HttpClient` instancias.

| Opción | Descripción | Default |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Obtiene o establece si `HttpClient` instancias deben seguir automáticamente las respuestas de redirección. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Obtiene o establece la dirección base del `HttpClient` instancias. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Obtiene o establece si `HttpClient` instancias deben controlar las cookies. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Obtiene o establece el número máximo de respuestas de redirección que `HttpClient` deben seguir las instancias. | 7 |

Crear el `WebApplicationFactoryClientOptions` clase y páselo a la [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (método) (valor predeterminado, los valores se muestran en el ejemplo de código):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Insertar servicios ficticios

Los servicios se pueden invalidar en una prueba con una llamada a [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) en el generador de host. **Para insertar servicios ficticios, debe tener el SUT un `Startup` clase con un `Startup.ConfigureServices` método.**

El ejemplo SUT incluye un servicio con ámbito que devuelve una cita. La oferta se incrusta en un campo oculto en la página de índice cuando se solicita la página de índice.

*Services/IQuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Páginas/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/Index.cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Cuando se ejecuta la aplicación SUT, se genera el marcado siguiente:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Para probar la inserción de la oferta y de servicio en una prueba de integración, un servicio de simulacro se inserta en el SUT por la prueba. El servicio ficticio reemplaza la aplicación `QuoteService` con un servicio proporcionado por la aplicación de prueba, llamado `TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices` se llama a, y se registra el servicio con ámbito:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

El marcado generado durante la ejecución de la prueba refleja el texto de oferta proporcionado por `TestQuoteService`, por lo tanto las fases de aserción:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Cómo la infraestructura de pruebas deduce la ruta de acceso de contenido raíz de aplicación

El `WebApplicationFactory` constructor infiere la ruta de acceso de contenido raíz de aplicación mediante la búsqueda de un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) en el ensamblado que contiene las pruebas de integración con una clave igual a la `TEntryPoint` ensamblado `System.Reflection.Assembly.FullName`. En caso de que no se encuentra un atributo con la clave correcta, `WebApplicationFactory` recurre a la búsqueda de un archivo de solución (*\*.sln*) y anexa el `TEntryPoint` nombre del ensamblado en el directorio de la solución. El directorio raíz de aplicación (la ruta de acceso raíz del contenido) se usa para detectar las vistas y los archivos de contenido.

En la mayoría de los casos, no es necesario establecer explícitamente la raíz de contenido de la aplicación, como la lógica de búsqueda busca normalmente la raíz de contenido correcta en tiempo de ejecución. En escenarios especiales donde no se encuentra la raíz de contenido mediante el algoritmo de búsqueda integradas, la aplicación de contenido raíz se puede especificar explícitamente o mediante el uso de lógica personalizada. Para establecer la raíz de contenido de la aplicación en esos escenarios, llame a la `UseSolutionRelativeContentRoot` método de extensión de la [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) paquete. Proporcione la ruta de acceso relativa de la solución y el patrón de nombre o glob del archivo de solución opcional (valor predeterminado = `*.sln`).

Llame a la [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) método de extensión mediante *una* de los métodos siguientes:

* Al configurar las clases de prueba con `WebApplicationFactory`, proporcionar una configuración personalizada con el [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

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

* Al configurar las clases de prueba con una personalizada `WebApplicationFactory`, heredan de `WebApplicationFactory` e invalidar [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

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

## <a name="disable-shadow-copying"></a>Deshabilitar las instantáneas

Las instantáneas, hace que las pruebas se ejecutan en una carpeta diferente a la carpeta de salida. Para que las pruebas para que funcione correctamente, deben deshabilitar las instantáneas. El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) usa xUnit y deshabilita la copia sombra de xUnit mediante la inclusión de un *xunit.runner.json* archivo con la opción de configuración correcto. Para obtener más información, consulte [configuración xUnit.net con JSON](https://xunit.github.io/docs/configuring-with-json.html).

Agregar el *xunit.runner.json* archivo a la raíz del proyecto de prueba con el siguiente contenido:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Ejemplo de pruebas de integración

El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) se compone de dos aplicaciones:

| Aplicación | Carpeta del proyecto | Descripción |
| --- | -------------- | ----------- |
| Mensaje de aplicación (SUT) | *src/RazorPagesProject* | Permite al usuario agregar, eliminar uno, elimine todo y analizar los mensajes. |
| Aplicación de prueba | *tests/RazorPagesProject.Tests* | Utilizado para la prueba de integración del SUT. |

Se pueden ejecutar las pruebas con las características integradas de prueba de un IDE, como [Visual Studio](https://www.visualstudio.com/vs/). Si usa [Visual Studio Code](https://code.visualstudio.com/) o la línea de comandos, ejecute el siguiente comando en un símbolo del sistema en el *tests/RazorPagesProject.Tests* carpeta:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organización de la aplicación (SUT) del mensaje

El SUT es un sistema de mensajes de las páginas de Razor con las siguientes características:

* La página de índice de la aplicación (*Pages/index.cshtml* y *Pages/Index.cshtml.cs*) proporciona una interfaz de usuario y la página de métodos de modelo para controlar la adición, eliminación y análisis de mensajes (palabras medios por mensaje) .
* Se describe un mensaje mediante la `Message` clase (*Data/Message.cs*) con dos propiedades: `Id` (clave) y `Text` (mensaje). El `Text` propiedad es necesaria y limitada a 200 caracteres.
* Los mensajes se almacenan mediante [base de datos de Entity Framework en memoria](/ef/core/providers/in-memory/)&#8224;.
* La aplicación contiene una capa de acceso a datos (DAL) en su clase de contexto de base de datos, `AppDbContext` (*Data/AppDbContext.cs*).
* Si la base de datos está vacía en el inicio de la aplicación, el almacén de mensajes se inicializa con tres mensajes.
* La aplicación incluye un `/SecurePage` que solo sean accesibles para un usuario autenticado.

&#8224;El tema EF, [pruebas con InMemory](/ef/core/miscellaneous/testing/in-memory), se explica cómo usar una base de datos en memoria para las pruebas con MSTest. Este tema se usa el [xUnit](https://xunit.github.io/) marco de pruebas. Los conceptos de pruebas e implementaciones de prueba a través de diferentes marcos son similares pero no idénticos.

Aunque la aplicación no usa el modelo de repositorio y no es un ejemplo eficaz de la [patrón de unidades de trabajo (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages admite estos patrones de desarrollo. Para obtener más información, consulte [diseñar la capa de persistencia de infraestructura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) y [lógica del controlador de pruebas](/aspnet/core/mvc/controllers/testing) (el ejemplo implementa el modelo de repositorio).

### <a name="test-app-organization"></a>Organización de la aplicación de prueba

La aplicación de prueba es una aplicación de consola en el *tests/RazorPagesProject.Tests* carpeta.

| Carpeta de la aplicación de prueba | Descripción |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* contiene métodos de prueba para el enrutamiento, obtener acceso a una página segura por un usuario no autenticado y obtener un perfil de usuario de GitHub y comprobación de inicio de sesión de usuario del perfil. |
| *IntegrationTests* | *IndexPageTests.cs* contiene las pruebas de integración de la página de índice mediante custom `WebApplicationFactory` clase. |
| *Las aplicaciones auxiliares y utilidades* | <ul><li>*Utilities.cs* contiene el `InitializeDbForTests` método utilizado para inicializar la base de datos con datos de prueba.</li><li>*HtmlHelpers.cs* proporciona un método para devolver un AngleSharp `IHtmlDocument` para su uso por los métodos de prueba.</li><li>*HttpClientExtensions.cs* proporcionan sobrecargas para `SendAsync` para enviar solicitudes al SUT.</li></ul> |

El marco de pruebas es [xUnit](https://xunit.github.io/). Las pruebas de integración se llevan a cabo mediante el [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), que incluye el [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Dado que el [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paquete se usa para configurar el servidor host y pruebas de prueba, el `TestHost` y `TestServer` paquetes no necesitan referencias de paquete directa en el archivo de proyecto de la aplicación de prueba o configuración de desarrollador de la aplicación de prueba.

**La propagación de la base de datos de prueba**

Las pruebas de integración suelen requieran un pequeño conjunto de datos en la base de datos antes de la ejecución de pruebas. Por ejemplo, una eliminación probar las llamadas para la eliminación de registros de base de datos, por lo que la base de datos debe tener al menos un registro para la solicitud de eliminación se realice correctamente.

La aplicación de ejemplo inicializa la base de datos con tres mensajes en *Utilities.cs* que las pruebas se pueden usar cuando ejecuta:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Recursos adicionales

* [Pruebas unitarias](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Pruebas unitarias de páginas de Razor](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Controladores de pruebas](xref:mvc/controllers/testing)
