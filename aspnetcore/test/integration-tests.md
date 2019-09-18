---
title: Pruebas de integración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo las pruebas de integración garantizan que los componentes de una aplicación, como la base de datos, el sistema de archivos y la red, funcionen correctamente en la infraestructura.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: test/integration-tests
ms.openlocfilehash: 272f0f2140647dd31319f8feada0ec04c7ab4e44
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082499"
---
# <a name="integration-tests-in-aspnet-core"></a>Pruebas de integración en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex) y [Steve Smith](https://ardalis.com/)

Las pruebas de integración aseguran que los componentes de una aplicación funcionan correctamente en un nivel que incluye la infraestructura de soporte de la aplicación, como la base de datos, el sistema de archivos y la red. ASP.NET Core admite las pruebas de integración con un marco de pruebas unitarias con un host Web de prueba y un servidor de prueba en memoria.

En este tema se da por supuesto un conocimiento básico de las pruebas unitarias. Si no está familiarizado con los conceptos de las pruebas, consulte el tema [pruebas unitarias en .net Core y .net Standard](/dotnet/core/testing/) y su contenido vinculado.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

La aplicación de ejemplo es una aplicación Razor Pages y asume un conocimiento básico de Razor Pages. Si no está familiarizado con Razor Pages, consulte los temas siguientes:

* [Introducción a las páginas de Razor](xref:razor-pages/index)
* [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Pruebas unitarias de páginas de Razor](xref:test/razor-pages-tests)

> [!NOTE]
> Para probar Spa, se recomienda usar una herramienta como [Selenium](https://www.seleniumhq.org/), que puede automatizar un explorador.

## <a name="introduction-to-integration-tests"></a>Introducción a las pruebas de integración

Las pruebas de integración evalúan los componentes de una aplicación en un nivel más amplio que las [pruebas unitarias](/dotnet/core/testing/). Las pruebas unitarias se usan para probar componentes de software aislados, como los métodos de clase individuales. Pruebas de integración confirme que dos o más componentes de aplicación funcionan juntos para generar un resultado esperado, lo que podría incluir todos los componentes necesarios para procesar completamente una solicitud.

Estas pruebas más amplias se usan para probar la infraestructura de la aplicación y todo el marco de trabajo, que a menudo incluyen los siguientes componentes:

* Base de datos
* Sistema de archivos
* Aplicaciones de red
* Canalización de solicitud-respuesta

Las pruebas unitarias usan componentes fabricados, conocidos como *falsificaciones* o *objetos ficticios*, en lugar de componentes de infraestructura.

A diferencia de las pruebas unitarias, las pruebas de integración:

* Use los componentes reales que usa la aplicación en producción.
* Requerir más código y procesamiento de datos.
* Tarda más tiempo en ejecutarse.

Por lo tanto, limite el uso de las pruebas de integración a los escenarios de infraestructura más importantes. Si un comportamiento se puede probar mediante una prueba unitaria o una prueba de integración, elija la prueba unitaria.

> [!TIP]
> No escriba pruebas de integración para cada posible permutación de datos y acceso a archivos con bases de datos y sistemas de archivos. Independientemente de cuántos lugares en una aplicación interactúen con las bases de datos y los sistemas de archivos, un conjunto centrado de pruebas de integración de lectura, escritura, actualización y eliminación suele ser capaz de probar adecuadamente los componentes del sistema de archivos y la base de datos. Utilice pruebas unitarias para pruebas rutinarias de la lógica de método que interactúan con estos componentes. En las pruebas unitarias, el uso de simulaciones y simulaciones de infraestructura produce una ejecución de prueba más rápida.

> [!NOTE]
> En los debates de las pruebas de integración, el proyecto probado se denomina frecuentemente *sistema en pruebas*o "SUT" para abreviar.

## <a name="aspnet-core-integration-tests"></a>Pruebas de integración de ASP.NET Core

Las pruebas de integración en ASP.NET Core requieren lo siguiente:

* Un proyecto de prueba se usa para contener y ejecutar las pruebas. El proyecto de prueba tiene una referencia al proyecto de ASP.NET Core probado, denominado *sistema sometido a prueba* (SUT). _"SUT" se usa en este tema para hacer referencia a la aplicación probada._
* El proyecto de prueba crea un host Web de prueba para SUT y utiliza un cliente de servidor de prueba para controlar las solicitudes y respuestas a SUT.
* Se utiliza un ejecutor de pruebas para ejecutar las pruebas y notificar los resultados de las pruebas.

Las pruebas de integración siguen una secuencia de eventos que incluyen los pasos habituales de *organización*, *acción*y *aserción* :

1. El host Web de SUT está configurado.
1. Se crea un cliente de servidor de prueba para enviar solicitudes a la aplicación.
1. Se ejecuta el paso de prueba *organizar* : La aplicación de prueba prepara una solicitud.
1. Se ejecuta el paso de prueba *Act* : El cliente envía la solicitud y recibe la respuesta.
1. Se ejecuta el paso de prueba *Assert* : La respuesta *real* se valida como *superada* o *no* superada en función de la respuesta *esperada* .
1. El proceso continúa hasta que se ejecutan todas las pruebas.
1. Se muestran los resultados de las pruebas.

Normalmente, el host Web de prueba se configura de manera diferente que el host web normal de la aplicación para las series de pruebas. Por ejemplo, puede usarse una base de datos diferente o una configuración de aplicación diferente para las pruebas.

Los componentes de infraestructura, como el host Web de prueba y el servidor de prueba en memoria ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), se proporcionan o administran mediante el paquete [Microsoft. AspNetCore. Mvc. Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) . El uso de este paquete simplifica la creación y ejecución de pruebas.

El `Microsoft.AspNetCore.Mvc.Testing` paquete controla las siguientes tareas:

* Copia el archivo de dependencias ( *\*. deps*) de SUT en el directorio *bin* del proyecto de prueba.
* Establece la raíz de contenido en la raíz del proyecto de SUT para que se encuentren los archivos estáticos y las páginas o vistas cuando se ejecutan las pruebas.
* Proporciona la clase [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) para agilizar el arranque de `TestServer`SUT con.

La documentación de [pruebas unitarias](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) describe cómo configurar un proyecto de prueba y un ejecutor de pruebas, junto con instrucciones detalladas sobre cómo ejecutar pruebas y recomendaciones para asignar nombres a las pruebas y las clases de prueba.

> [!NOTE]
> Al crear un proyecto de prueba para una aplicación, separe las pruebas unitarias de las pruebas de integración en proyectos diferentes. Esto ayuda a garantizar que los componentes de prueba de la infraestructura no se incluyan accidentalmente en las pruebas unitarias. La separación de las pruebas unitarias y de integración también permite controlar qué conjunto de pruebas se ejecutan.

Prácticamente no hay ninguna diferencia entre la configuración de las pruebas de aplicaciones de Razor Pages y de MVC. La única diferencia radica en cómo se denominan las pruebas. En una aplicación Razor pages, las pruebas de los puntos de conexión de página normalmente se denominan después de la clase `IndexPageTests` de modelo de página (por ejemplo, para probar la integración de componentes para la página de índice). En una aplicación de MVC, las pruebas suelen estar organizadas por clases de controlador y denominadas después de los controladores `HomeControllerTests` que prueban (por ejemplo, para probar la integración de componentes para el controlador de inicio).

## <a name="test-app-prerequisites"></a>Probar los requisitos previos de la aplicación

El proyecto de prueba debe:

* Haga referencia a los siguientes paquetes:
  * [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Especifique el SDK Web en el archivo de proyecto`<Project Sdk="Microsoft.NET.Sdk.Web">`(). El SDK web es necesario cuando se hace referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Estos requisitos previos se pueden consultar en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/). Inspeccione el archivo *tests/RazorPagesProject. tests/RazorPagesProject. tests. csproj* . La aplicación de ejemplo usa el marco de pruebas de [xUnit](https://xunit.github.io/) y la biblioteca de analizador de [AngleSharp](https://anglesharp.github.io/) , por lo que la aplicación de ejemplo también hace referencia a:

* [xunit](https://www.nuget.org/packages/xunit/)
* [xunit.runner.visualstudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a>Entorno de SUT

Si no se establece el [entorno](xref:fundamentals/environments) de SUT, el entorno tiene como valor predeterminado el desarrollo.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Pruebas básicas con el valor predeterminado de WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) se usa para crear un [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) para las pruebas de integración. `TEntryPoint`es la clase de punto de entrada de SUT, normalmente `Startup` la clase.

Las clases de prueba implementan una interfaz de *accesorio de clase* ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) para indicar que la clase contiene pruebas y proporcionan instancias de objetos compartidos en las pruebas de la clase.

### <a name="basic-test-of-app-endpoints"></a>Prueba básica de los puntos de conexión de la aplicación

La siguiente clase de prueba `BasicTests`,, `WebApplicationFactory` utiliza para arrancar el SUT y proporcionar un [HttpClient](/dotnet/api/system.net.http.httpclient) a un método de prueba `Get_EndpointsReturnSuccessAndCorrectContentType`,. El método comprueba si el código de estado de la respuesta es correcto (códigos de estado en el intervalo `Content-Type` 200-299) `text/html; charset=utf-8` y el encabezado es para varias páginas de la aplicación.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crea una instancia de `HttpClient` que sigue automáticamente las redirecciones y controla las cookies.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

De forma predeterminada, las cookies no esenciales no se conservan entre solicitudes cuando la [Directiva de consentimiento de RGPD](xref:security/gdpr) está habilitada. Para conservar las cookies no esenciales, como las usadas por el proveedor TempData, márquela como esenciales en las pruebas. Para obtener instrucciones sobre cómo marcar una cookie como esencial, vea [cookies esenciales](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Probar un extremo seguro

Otra prueba de la `BasicTests` clase comprueba que un punto de conexión seguro redirige a un usuario no autenticado a la página de inicio de sesión de la aplicación.

En SUT, la `/SecurePage` página usa una Convención [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) para aplicar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página. Para obtener más información, vea [Razor pages convenciones de autorización](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

En la `Get_SecurePageRequiresAnAuthenticatedUser` prueba, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) se establece en Disallow redirects estableciendo [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) en `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Al no permitir que el cliente siga la redirección, se pueden realizar las siguientes comprobaciones:

* El código de estado devuelto por SUT puede comprobarse con el resultado esperado de [HttpStatusCode. Redirect](/dotnet/api/system.net.httpstatuscode) , no con el código de estado final después de la redirección a la página de inicio de sesión, que sería [HttpStatusCode. OK](/dotnet/api/system.net.httpstatuscode).
* Se `Location` comprueba el valor de encabezado de los encabezados de respuesta para confirmar que empieza por `http://localhost/Identity/Account/Login`, no la respuesta de la página de inicio de `Location` sesión final, donde el encabezado no estaría presente.

Para obtener más información `WebApplicationFactoryClientOptions`sobre, vea la sección [Opciones de cliente](#client-options) .

## <a name="customize-webapplicationfactory"></a>Personalización de WebApplicationFactory

La configuración del host Web se puede crear independientemente de las clases de prueba heredando de `WebApplicationFactory` para crear uno o varios generadores personalizados:

1. Heredar `WebApplicationFactory` de e invalidar [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) permite la configuración de la colección de servicios con [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   El método realiza la `InitializeDbForTests` propagación de la base de datos en la aplicación de [ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) . El método se describe en el [ejemplo de pruebas de integración: Sección de la](#test-app-organization) organización de la aplicación de prueba.

2. Utilice las clases `CustomWebApplicationFactory` de prueba personalizadas. En el ejemplo siguiente se usa el generador `IndexPageTests` en la clase:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   El cliente de la aplicación de ejemplo está configurado para `HttpClient` evitar que el siguiente redireccionamientos. Como se explica en la sección [probar un punto de conexión seguro](#test-a-secure-endpoint) , esto permite que las pruebas comprueben el resultado de la primera respuesta de la aplicación. La primera respuesta es una redirección en muchas de estas pruebas con un `Location` encabezado.

3. Una prueba típica usa los `HttpClient` métodos auxiliares y para procesar la solicitud y la respuesta:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Cualquier solicitud POST a SUT debe cumplir la comprobación antifalsificación realizada automáticamente por el [sistema antifalsificación de protección de datos](xref:security/data-protection/introduction)de la aplicación. Para organizar la solicitud POST de una prueba, la aplicación de prueba debe:

1. Realice una solicitud para la página.
1. Analice la cookie antifalsificación y solicite el token de validación de la respuesta.
1. Realice la solicitud POST con la cookie antifalsificación y el token de validación de solicitudes en su lugar.

Los `SendAsync` métodos de extensión auxiliares (*helpers/HttpClientExtensions. CS*) y el `GetDocumentAsync` método auxiliar (*helpers/HtmlHelpers. CS*) en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) usan el analizador de [AngleSharp](https://anglesharp.github.io/) para controlar la antifalsificación. Compruebe con los métodos siguientes:

* `GetDocumentAsync`Recibe el [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) y devuelve un `IHtmlDocument`. &ndash; `GetDocumentAsync`utiliza un generador que prepara una *respuesta virtual* basada en el original `HttpResponseMessage`. Para obtener más información, vea la [documentación de AngleSharp](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync`los métodos de extensión `HttpClient` para componen un [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) y llaman a [SendAsync (HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) para enviar solicitudes a SUT. Las sobrecargas `SendAsync` de aceptan el formulario HTML`IHtmlFormElement`() y lo siguiente:
  * Botón Enviar del formulario (`IHtmlElement`)
  * Colección de valores de`IEnumerable<KeyValuePair<string, string>>`formulario ()
  * Botón de envío`IHtmlElement`() y valores de`IEnumerable<KeyValuePair<string, string>>`formulario ()

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) es una biblioteca de análisis de terceros que se usa con fines de demostración en este tema y la aplicación de ejemplo. AngleSharp no es compatible o es necesario para la prueba de integración de aplicaciones ASP.NET Core. Se pueden usar otros analizadores, como el [paquete de agilidad HTML (HAP)](https://html-agility-pack.net/). Otro enfoque consiste en escribir código para controlar el token de comprobación de solicitudes y la cookie antifalsificación del sistema antifalsificación directamente.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personalización del cliente con WithWebHostBuilder

Cuando se requiere una configuración adicional dentro de un método de prueba, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crea un nuevo `WebApplicationFactory` con un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) que se ha personalizado aún más mediante la configuración.

El `Post_DeleteMessageHandler_ReturnsRedirectToRoot` método de prueba de la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) muestra el `WithWebHostBuilder`uso de. Esta prueba realiza una eliminación de registros en la base de datos desencadenando un envío de formulario en el SUT.

Dado que otra prueba de `IndexPageTests` la clase realiza una operación que elimina todos los registros de la base de datos y puede ejecutarse `Post_DeleteMessageHandler_ReturnsRedirectToRoot` antes que el método, la base de datos se propaga en este método de prueba para asegurarse de que un registro está presente para que se elimine el SUT. La selección del `messages`botóndel formulario en el SUT se simula en la solicitud a SUT: `deleteBtn1`

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opciones de cliente

En la tabla siguiente se muestra el [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) predeterminado disponible `HttpClient` al crear instancias de.

| Opción | DESCRIPCIÓN | Valor predeterminado |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Obtiene o establece si `HttpClient` las instancias de deben seguir automáticamente las respuestas de redirección. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Obtiene o establece la dirección base de `HttpClient` las instancias de. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Obtiene o establece si `HttpClient` las instancias de deben controlar las cookies. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Obtiene o establece el número máximo de respuestas de redirección `HttpClient` que deben seguir las instancias. | 7 |

Cree la `WebApplicationFactoryClientOptions` clase y pásela al método [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (los valores predeterminados se muestran en el ejemplo de código):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Inyectar servicios ficticios

Los servicios se pueden invalidar en una prueba con una llamada a [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) en el generador de hosts. **Para inyectar servicios ficticios, SUT debe tener `Startup` una clase con `Startup.ConfigureServices` un método.**

El SUT de ejemplo incluye un servicio de ámbito que devuelve una comilla. La oferta se incrusta en un campo oculto en la página de índice cuando se solicita la página de índice.

*Services/IQuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Páginas/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/index. CS*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Cuando se ejecuta la aplicación SUT, se genera el siguiente marcado:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Para probar la inyección de servicio y de Comillas en una prueba de integración, la prueba inserta un servicio ficticio en el SUT. El servicio ficticio reemplaza la aplicación `QuoteService` por un servicio proporcionado por la aplicación de prueba, llamado: `TestQuoteService`

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices`se llama a y se registra el servicio de ámbito:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

El marcado producido durante la ejecución de la prueba refleja el texto de la `TestQuoteService`cita proporcionado por, por lo que la aserción pasa:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Cómo la infraestructura de prueba deduce la ruta de acceso raíz del contenido de la aplicación

El `WebApplicationFactory` constructor deduce la ruta de acceso raíz del contenido de la aplicación buscando un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) en el ensamblado que contiene las pruebas de integración `TEntryPoint` con `System.Reflection.Assembly.FullName`una clave igual al ensamblado. En caso de que no se encuentre un atributo con la `WebApplicationFactory` clave correcta, recurre a la búsqueda de un archivo de solución ( *\*. sln*) y anexa el nombre del `TEntryPoint` ensamblado al directorio de la solución. El directorio raíz de la aplicación (la ruta de acceso raíz del contenido) se usa para detectar los archivos de contenido y las vistas.

## <a name="disable-shadow-copying"></a>Deshabilitar la copia sombra

La copia sombra hace que las pruebas se ejecuten en un directorio diferente que el directorio de salida. Para que las pruebas funcionen correctamente, se debe deshabilitar la copia sombra. La [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) usa xUnit y deshabilita la copia sombra de xUnit mediante la inclusión de un archivo *xUnit. Runner. JSON* con la opción de configuración correcta. Para obtener más información, consulte [configuración de xUnit con JSON](https://xunit.github.io/docs/configuring-with-json.html).

Agregue el archivo *xUnit. Runner. JSON* a la raíz del proyecto de prueba con el siguiente contenido:

```json
{
  "shadowCopy": false
}
```

Si usa Visual Studio, establezca la propiedad **Copiar en el directorio de salida** del archivo en **copiar siempre**. Si no usa Visual Studio, agregue un `Content` destino al archivo de proyecto de la aplicación de prueba:

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a>Eliminación de objetos

Una vez ejecutadas las `IClassFixture` pruebas de la implementación, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) y [HttpClient](/dotnet/api/system.net.http.httpclient) se eliminan cuando xUnit desecha [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1). Si los objetos a los que se ha creado una instancia del desarrollador requieren su `IClassFixture` eliminación, desechen en la implementación. Para obtener más información, vea [implementar un método Dispose](/dotnet/standard/garbage-collection/implementing-dispose).

## <a name="integration-tests-sample"></a>Ejemplo de pruebas de integración

La [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) se compone de dos aplicaciones:

| Aplicación | Directorio del proyecto | DESCRIPCIÓN |
| --- | ----------------- | ----------- |
| Aplicación de mensajes (SUT) | *src/RazorPagesProject* | Permite al usuario agregar, eliminar y analizar todos los mensajes. |
| Aplicación de prueba | *tests/RazorPagesProject.Tests* | Se usa para probar la integración de SUT. |

Las pruebas se pueden ejecutar con las características de prueba integradas de un IDE, como [Visual Studio](https://visualstudio.microsoft.com). Si usa [Visual Studio Code](https://code.visualstudio.com/) o la línea de comandos, ejecute el siguiente comando en un símbolo del sistema en el directorio *tests/RazorPagesProject. tests* :

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a>Organización de mensajes (SUT)

SUT es un sistema de mensajes Razor Pages con las siguientes características:

* La página de índice de la aplicación (*pages/index. cshtml* y *pages/index. cshtml. CS*) proporciona un método de interfaz de usuario y un modelo de página para controlar la adición, eliminación y análisis de mensajes (promedio de palabras por mensaje).
* La `Message` clase (*Data/Message. CS*) describe un mensaje con dos propiedades: `Id` (Key) y `Text` (Message). La `Text` propiedad es necesaria y está limitada a 200 caracteres.
* Los mensajes se almacenan mediante&#8224; [Entity Framework base de datos en memoria](/ef/core/providers/in-memory/).
* La aplicación contiene una capa de acceso a datos (dal) en su clase de `AppDbContext` contexto de base de datos, (*Data/AppDbContext. CS*).
* Si la base de datos está vacía al iniciarse la aplicación, el almacén de mensajes se inicializa con tres mensajes.
* La aplicación incluye un `/SecurePage` al que solo puede tener acceso un usuario autenticado.

&#8224;En el tema EF, [Test with inmemory](/ef/core/miscellaneous/testing/in-memory), se explica cómo usar una base de datos en memoria para las pruebas con MSTest. En este tema se usa el marco de pruebas de [xUnit](https://xunit.github.io/) . Los conceptos de prueba y las implementaciones de prueba en diferentes marcos de pruebas son similares, pero no idénticos.

Aunque la aplicación no usa el patrón de repositorio y no es un ejemplo eficaz del [patrón de unidad de trabajo (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor pages admite estos patrones de desarrollo. Para obtener más información, consulte [diseño de la capa de persistencia de la infraestructura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) y la [lógica del controlador de pruebas](/aspnet/core/mvc/controllers/testing) (el ejemplo implementa el patrón del repositorio).

### <a name="test-app-organization"></a>Probar la organización de la aplicación

La aplicación de prueba es una aplicación de consola dentro del directorio *tests/RazorPagesProject. tests* .

| Directorio de la aplicación de prueba | DESCRIPCIÓN |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.CS* contiene métodos de prueba para el enrutamiento, el acceso a una página segura por parte de un usuario no autenticado y la obtención de un perfil de usuario de Github y la comprobación del inicio de sesión del usuario del perfil. |
| *IntegrationTests* | *IndexPageTests.CS* contiene las pruebas de integración para la página de índice `WebApplicationFactory` mediante la clase personalizada. |
| *Aplicaciones auxiliares y utilidades* | <ul><li>*Utilities.CS* contiene el `InitializeDbForTests` método que se usa para inicializar la base de datos con datos de prueba.</li><li>*HtmlHelpers.CS* proporciona un método para devolver un AngleSharp `IHtmlDocument` para que lo usen los métodos de prueba.</li><li>*HttpClientExtensions.CS* proporcionan sobrecargas para `SendAsync` que envíen solicitudes a SUT.</li></ul> |

El marco de pruebas es [xUnit](https://xunit.github.io/). Las pruebas de integración se realizan mediante [Microsoft. AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost), que incluye [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Dado que el paquete [Microsoft. AspNetCore. Mvc. Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) se usa para configurar el host de prueba y el servidor `TestHost` de `TestServer` prueba, los paquetes y no requieren referencias de paquete directas en el archivo de proyecto o desarrollador de la aplicación de prueba. configuración en la aplicación de prueba.

**Propagación de la base de datos para pruebas**

Las pruebas de integración suelen requerir un conjunto de datos pequeño en la base de datos antes de la ejecución de la prueba. Por ejemplo, una prueba de eliminación llama a para la eliminación de un registro de base de datos, por lo que la base de datos debe tener al menos un registro para que la solicitud de eliminación se realice correctamente.

La aplicación de ejemplo inicializa la base de datos con tres mensajes en *Utilities.CS* que las pruebas pueden usar cuando se ejecutan:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Recursos adicionales

* [Pruebas unitarias](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Pruebas unitarias de páginas de Razor](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Controladores de pruebas](xref:mvc/controllers/testing)
