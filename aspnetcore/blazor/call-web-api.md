---
title: Llamada a una API Web desde ASP.NET Core extraordinarias
author: guardrex
description: Obtenga información sobre cómo llamar a una API Web desde una aplicación increíblemente con aplicaciones auxiliares de JSON, incluida la realización de solicitudes de uso compartido de recursos entre orígenes (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/call-web-api
ms.openlocfilehash: ea819da57c382b724098c55f2f799d7deea363f2
ms.sourcegitcommit: 4649814d1ae32248419da4e8f8242850fd8679a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "71975641"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="3a90a-103">Llamada a una API Web desde ASP.NET Core extraordinarias</span><span class="sxs-lookup"><span data-stu-id="3a90a-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="3a90a-104">Por [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)y [Juan de la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="3a90a-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3a90a-105">Las aplicaciones de webassembly increíbles llaman a las API Web mediante un servicio @no__t preconfigurado-0.</span><span class="sxs-lookup"><span data-stu-id="3a90a-105">Blazor WebAssembly apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="3a90a-106">Cree solicitudes, que pueden incluir opciones de [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) de JavaScript, con aplicaciones auxiliares JSON más increíbles o con <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="3a90a-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="3a90a-107">Las aplicaciones de servidor increíbles llaman a las API Web mediante instancias de <xref:System.Net.Http.HttpClient> creadas normalmente con <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="3a90a-107">Blazor Server apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="3a90a-108">Para obtener más información, consulta <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="3a90a-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="3a90a-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3a90a-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3a90a-110">Para obtener ejemplos de webassembly más increíbles, vea los siguientes componentes en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3a90a-110">For Blazor WebAssembly examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="3a90a-111">Llamar a la API Web (*pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="3a90a-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="3a90a-112">Evaluador de solicitudes HTTP (*Components/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="3a90a-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="3a90a-113">Aplicaciones auxiliares de HttpClient y JSON</span><span class="sxs-lookup"><span data-stu-id="3a90a-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="3a90a-114">En las aplicaciones de webassembly increíbles, [HttpClient](xref:fundamentals/http-requests) está disponible como un servicio preconfigurado para realizar solicitudes de vuelta al servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="3a90a-114">In Blazor WebAssembly apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="3a90a-115">Para usar las aplicaciones auxiliares JSON `HttpClient`, agregue una referencia de paquete a `Microsoft.AspNetCore.Blazor.HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3a90a-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="3a90a-116">las aplicaciones auxiliares `HttpClient` y JSON también se usan para llamar a puntos de conexión de API Web de terceros.</span><span class="sxs-lookup"><span data-stu-id="3a90a-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="3a90a-117">`HttpClient` se implementa mediante la [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) del explorador y está sujeto a sus limitaciones, incluido el cumplimiento de la misma directiva de origen.</span><span class="sxs-lookup"><span data-stu-id="3a90a-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="3a90a-118">La dirección base del cliente se establece en la dirección del servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="3a90a-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="3a90a-119">Inserte una instancia de `HttpClient` mediante la Directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="3a90a-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="3a90a-120">En los ejemplos siguientes, una API Web de todo procesa operaciones de creación, lectura, actualización y eliminación (CRUD).</span><span class="sxs-lookup"><span data-stu-id="3a90a-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="3a90a-121">Los ejemplos se basan en una clase `TodoItem` que almacena:</span><span class="sxs-lookup"><span data-stu-id="3a90a-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="3a90a-122">ID. (`Id`, `long`) &ndash; ID. único del elemento.</span><span class="sxs-lookup"><span data-stu-id="3a90a-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="3a90a-123">Nombre (`Name`, `string`) &ndash; nombre del elemento.</span><span class="sxs-lookup"><span data-stu-id="3a90a-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="3a90a-124">Estado (`IsComplete`, `bool`) &ndash; Si el elemento todo ha finalizado.</span><span class="sxs-lookup"><span data-stu-id="3a90a-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="3a90a-125">Los métodos auxiliares de JSON envían solicitudes a un URI (una API Web en los ejemplos siguientes) y procesan la respuesta:</span><span class="sxs-lookup"><span data-stu-id="3a90a-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="3a90a-126">`GetJsonAsync` &ndash; envía una solicitud HTTP GET y analiza el cuerpo de respuesta JSON para crear un objeto.</span><span class="sxs-lookup"><span data-stu-id="3a90a-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="3a90a-127">En el código siguiente, el componente muestra el `_todoItems`.</span><span class="sxs-lookup"><span data-stu-id="3a90a-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="3a90a-128">El método `GetTodoItems` se desencadena cuando finaliza la representación del componente ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span><span class="sxs-lookup"><span data-stu-id="3a90a-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="3a90a-129">Consulte la aplicación de ejemplo para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="3a90a-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="3a90a-130">`PostJsonAsync` &ndash; envía una solicitud HTTP POST, incluido el contenido con codificación JSON, y analiza el cuerpo de respuesta JSON para crear un objeto.</span><span class="sxs-lookup"><span data-stu-id="3a90a-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="3a90a-131">En el código siguiente, se proporciona `_newItemName` mediante un elemento enlazado del componente.</span><span class="sxs-lookup"><span data-stu-id="3a90a-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="3a90a-132">El método `AddItem` se desencadena seleccionando un elemento `<button>`.</span><span class="sxs-lookup"><span data-stu-id="3a90a-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="3a90a-133">Consulte la aplicación de ejemplo para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="3a90a-133">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostJsonAsync("api/TodoItems", addItem);
      }
  }
  ```

* <span data-ttu-id="3a90a-134">`PutJsonAsync` &ndash; envía una solicitud HTTP PUT, incluido el contenido con codificación JSON.</span><span class="sxs-lookup"><span data-stu-id="3a90a-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="3a90a-135">En el código siguiente, los elementos enlazados del componente proporcionan valores `_editItem` para `Name` y `IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="3a90a-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="3a90a-136">El @no__t del elemento se establece cuando el elemento está seleccionado en otra parte de la interfaz de usuario y se llama a `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="3a90a-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="3a90a-137">El método `SaveItem` se desencadena seleccionando el elemento Save `<button>`.</span><span class="sxs-lookup"><span data-stu-id="3a90a-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="3a90a-138">Consulte la aplicación de ejemplo para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="3a90a-138">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input type="checkbox" @bind="_editItem.IsComplete" />
  <input @bind="_editItem.Name" />
  <button @onclick="@SaveItem">Save</button>

  @code {
      private TodoItem _editItem = new TodoItem();

      private void EditItem(long id)
      {
          var editItem = _todoItems.Single(i => i.Id == id);
          _editItem = new TodoItem { Id = editItem.Id, Name = editItem.Name, 
              IsComplete = editItem.IsComplete };
      }

      private async Task SaveItem() =>
          await Http.PutJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="3a90a-139"><xref:System.Net.Http> incluye métodos de extensión adicionales para enviar solicitudes HTTP y recibir respuestas HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a90a-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="3a90a-140">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) se usa para enviar una solicitud HTTP DELETE a una API Web.</span><span class="sxs-lookup"><span data-stu-id="3a90a-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="3a90a-141">En el código siguiente, el elemento Delete `<button>` llama al método `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="3a90a-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="3a90a-142">El elemento `<input>` enlazado proporciona el `id` del elemento que se va a eliminar.</span><span class="sxs-lookup"><span data-stu-id="3a90a-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="3a90a-143">Consulte la aplicación de ejemplo para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="3a90a-143">See the sample app for a complete example.</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/TodoItems/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="3a90a-144">Uso compartido de recursos entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="3a90a-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="3a90a-145">La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que sirvió a la Página Web.</span><span class="sxs-lookup"><span data-stu-id="3a90a-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="3a90a-146">Esta restricción se llama *Directiva del mismo origen*.</span><span class="sxs-lookup"><span data-stu-id="3a90a-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="3a90a-147">La Directiva del mismo origen impide que un sitio malintencionado Lea datos confidenciales de otro sitio.</span><span class="sxs-lookup"><span data-stu-id="3a90a-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="3a90a-148">Para realizar solicitudes desde el explorador a un punto de conexión con un origen diferente, el *punto de conexión* debe habilitar el [uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="3a90a-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="3a90a-149">En la aplicación de ejemplo se muestra el uso de CORS en el componente Call Web API (*pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="3a90a-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="3a90a-150">Para permitir que otros sitios realicen solicitudes de uso compartido de recursos entre orígenes (CORS) a la aplicación, consulte <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="3a90a-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="3a90a-151">HttpClient y HttpRequestMessage con las opciones de solicitud de la API fetch</span><span class="sxs-lookup"><span data-stu-id="3a90a-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="3a90a-152">Cuando se ejecute en webassembly en una aplicación de webassembly increíblemente alta, use [HttpClient](xref:fundamentals/http-requests) y <xref:System.Net.Http.HttpRequestMessage> para personalizar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3a90a-152">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="3a90a-153">Por ejemplo, puede especificar el URI de solicitud, el método HTTP y cualquier encabezado de solicitud deseado.</span><span class="sxs-lookup"><span data-stu-id="3a90a-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="3a90a-154">Proporcione opciones de solicitud a la [API de captura](https://developer.mozilla.org/docs/Web/API/Fetch_API) de JavaScript subyacente mediante la propiedad `WebAssemblyHttpMessageHandler.FetchArgs` en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="3a90a-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="3a90a-155">Como se muestra en el ejemplo siguiente, la propiedad `credentials` se establece en cualquiera de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="3a90a-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="3a90a-156">`FetchCredentialsOption.Include` ("include") &ndash; aconseja al explorador que envíe las credenciales (como cookies o encabezados de autenticación HTTP) incluso para las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="3a90a-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="3a90a-157">Solo se permite cuando la Directiva CORS está configurada para permitir credenciales.</span><span class="sxs-lookup"><span data-stu-id="3a90a-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="3a90a-158">`FetchCredentialsOption.Omit` ("omitir") &ndash; informa al explorador de que nunca debe enviar credenciales (como cookies o encabezados de autenticación HTTP).</span><span class="sxs-lookup"><span data-stu-id="3a90a-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="3a90a-159">`FetchCredentialsOption.SameOrigin` ("Same-Origin") &ndash; aconseja al explorador que envíe las credenciales (como cookies o encabezados de autenticación HTTP) solo si la dirección URL de destino está en el mismo origen que la aplicación que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="3a90a-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

```cshtml
@using System.Net.Http
@using System.Net.Http.Headers
@inject HttpClient Http

@code {
    private async Task PostRequest()
    {
        Http.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", "{OAUTH TOKEN}");

        var requestMessage = new HttpRequestMessage()
        {
            Method = new HttpMethod("POST"),
            RequestUri = new Uri("https://localhost:10000/api/TodoItems"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");
        
        requestMessage.Properties[WebAssemblyHttpMessageHandler.FetchArgs] = new
        { 
            credentials = FetchCredentialsOption.Include
        };

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="3a90a-160">Para obtener más información sobre las opciones de la API de captura, vea [MDN web docs: WindowOrWorkerGlobalScope. fetch ():P arameters @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="3a90a-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="3a90a-161">Al enviar credenciales (cookies de autorización/encabezados) en solicitudes de CORS, la Directiva de CORS debe permitir el encabezado `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="3a90a-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="3a90a-162">La siguiente directiva incluye la configuración de:</span><span class="sxs-lookup"><span data-stu-id="3a90a-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="3a90a-163">Orígenes de la solicitud (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="3a90a-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="3a90a-164">Cualquier método (verbo).</span><span class="sxs-lookup"><span data-stu-id="3a90a-164">Any method (verb).</span></span>
* <span data-ttu-id="3a90a-165">`Content-Type` y `Authorization` encabezados.</span><span class="sxs-lookup"><span data-stu-id="3a90a-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="3a90a-166">Para permitir un encabezado personalizado (por ejemplo, `x-custom-header`), enumere el encabezado al llamar a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="3a90a-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="3a90a-167">Credenciales establecidas por el código JavaScript del lado cliente (propiedad `credentials` establecida en `include`).</span><span class="sxs-lookup"><span data-stu-id="3a90a-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="3a90a-168">Para obtener más información, vea <xref:security/cors> y el componente del evaluador de solicitudes HTTP de la aplicación de ejemplo (*Components/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="3a90a-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3a90a-169">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3a90a-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="3a90a-170">Configuración del punto de conexión HTTPS de Kestrel</span><span class="sxs-lookup"><span data-stu-id="3a90a-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="3a90a-171">Uso compartido de recursos entre orígenes (CORS) en W3C</span><span class="sxs-lookup"><span data-stu-id="3a90a-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
