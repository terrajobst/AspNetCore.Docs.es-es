---
title: Llamar a una API Web desde ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre cómo llamar a una API Web desde una aplicación Blazor mediante aplicaciones auxiliares de JSON, incluida la realización de solicitudes de uso compartido de recursos entre orígenes (CORS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: 345fb6962e3376c22551eb7914c70c89cb7100d5
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213280"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="238a4-103">Llamar a una API Web desde ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="238a4-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="238a4-104">Por [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)y [Juan de la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="238a4-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="238a4-105">[Blazor aplicaciones Webassembly](xref:blazor/hosting-models#blazor-webassembly) llaman a las API Web mediante un servicio `HttpClient` preconfigurado.</span><span class="sxs-lookup"><span data-stu-id="238a4-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="238a4-106">Cree solicitudes, que pueden incluir opciones de [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) de JavaScript, con Blazor aplicaciones auxiliares de JSON o con <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="238a4-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span> <span data-ttu-id="238a4-107">El servicio `HttpClient` en Blazor aplicaciones webassembly se centra en realizar solicitudes de vuelta al servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="238a4-107">The `HttpClient` service in Blazor WebAssembly apps is focused on making requests back to the server of origin.</span></span> <span data-ttu-id="238a4-108">Las instrucciones de este tema solo se aplican a Blazor aplicaciones webassembly.</span><span class="sxs-lookup"><span data-stu-id="238a4-108">The guidance in this topic only pertains to Blazor WebAssembly apps.</span></span>

<span data-ttu-id="238a4-109">las aplicaciones de [Blazor Server](xref:blazor/hosting-models#blazor-server) llaman a las API Web mediante instancias de <xref:System.Net.Http.HttpClient>, creadas normalmente con <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="238a4-109">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances, typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="238a4-110">Las instrucciones de este tema no pertenecen a las aplicaciones de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="238a4-110">The guidance in this topic doesn't pertain to Blazor Server apps.</span></span> <span data-ttu-id="238a4-111">Al desarrollar aplicaciones de Blazor Server, siga las instrucciones de <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="238a4-111">When developing Blazor Server apps, follow the guidance in <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="238a4-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargar](xref:index#how-to-download-a-sample)) &ndash; seleccione la aplicación *BlazorWebAssemblySample* .</span><span class="sxs-lookup"><span data-stu-id="238a4-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="238a4-113">Vea los siguientes componentes en la aplicación de ejemplo *BlazorWebAssemblySample* :</span><span class="sxs-lookup"><span data-stu-id="238a4-113">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="238a4-114">Llamar a la API Web (*pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="238a4-114">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="238a4-115">Evaluador de solicitudes HTTP (*Components/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="238a4-115">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="238a4-116">Paquetes</span><span class="sxs-lookup"><span data-stu-id="238a4-116">Packages</span></span>

<span data-ttu-id="238a4-117">Haga referencia al [Microsoft. AspNetCore.Blazorexperimental. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/)Paquete de NuGet de HttpClient en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="238a4-117">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="238a4-118">`Microsoft.AspNetCore.Blazor.HttpClient` se basa en `HttpClient` y [System. Text. JSON](https://www.nuget.org/packages/System.Text.Json/).</span><span class="sxs-lookup"><span data-stu-id="238a4-118">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="238a4-119">Para usar una API estable, use el paquete [Microsoft. Aspnet. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , que usa [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/)/[JSON.net](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="238a4-119">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="238a4-120">El uso de la API estable en `Microsoft.AspNet.WebApi.Client` no proporciona las aplicaciones auxiliares de JSON que se describen en este tema, que son únicas del paquete experimental `Microsoft.AspNetCore.Blazor.HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="238a4-120">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="238a4-121">Aplicaciones auxiliares de HttpClient y JSON</span><span class="sxs-lookup"><span data-stu-id="238a4-121">HttpClient and JSON helpers</span></span>

<span data-ttu-id="238a4-122">En una aplicación Blazor webassembly, [HttpClient](xref:fundamentals/http-requests) está disponible como un servicio preconfigurado para realizar solicitudes de vuelta al servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="238a4-122">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="238a4-123">De forma predeterminada, una aplicación de Blazor Server no incluye un servicio `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="238a4-123">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="238a4-124">Proporcione una `HttpClient` a la aplicación mediante la [infraestructura de factoría HttpClient](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="238a4-124">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="238a4-125">las aplicaciones auxiliares de `HttpClient` y JSON también se usan para llamar a puntos de conexión de API Web de terceros.</span><span class="sxs-lookup"><span data-stu-id="238a4-125">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="238a4-126">`HttpClient` se implementa mediante la [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) del explorador y está sujeto a sus limitaciones, incluido el cumplimiento de la misma directiva de origen.</span><span class="sxs-lookup"><span data-stu-id="238a4-126">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="238a4-127">La dirección base del cliente se establece en la dirección del servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="238a4-127">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="238a4-128">Inserte una instancia de `HttpClient` mediante la Directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="238a4-128">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="238a4-129">En los ejemplos siguientes, una API Web de todo procesa operaciones de creación, lectura, actualización y eliminación (CRUD).</span><span class="sxs-lookup"><span data-stu-id="238a4-129">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="238a4-130">Los ejemplos se basan en una clase `TodoItem` que almacena:</span><span class="sxs-lookup"><span data-stu-id="238a4-130">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="238a4-131">ID. (`Id`, `long`) &ndash; identificador único del elemento.</span><span class="sxs-lookup"><span data-stu-id="238a4-131">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="238a4-132">Nombre (`Name`, `string`) &ndash; nombre del elemento.</span><span class="sxs-lookup"><span data-stu-id="238a4-132">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="238a4-133">Estado (`IsComplete`, `bool`) &ndash; indicación si el elemento todo ha finalizado.</span><span class="sxs-lookup"><span data-stu-id="238a4-133">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="238a4-134">Los métodos auxiliares de JSON envían solicitudes a un URI (una API Web en los ejemplos siguientes) y procesan la respuesta:</span><span class="sxs-lookup"><span data-stu-id="238a4-134">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="238a4-135">`GetJsonAsync` &ndash; envía una solicitud HTTP GET y analiza el cuerpo de respuesta JSON para crear un objeto.</span><span class="sxs-lookup"><span data-stu-id="238a4-135">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="238a4-136">En el código siguiente, el componente muestra el `_todoItems`.</span><span class="sxs-lookup"><span data-stu-id="238a4-136">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="238a4-137">El método `GetTodoItems` se desencadena cuando finaliza la representación del componente ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span><span class="sxs-lookup"><span data-stu-id="238a4-137">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="238a4-138">Consulte la aplicación de ejemplo para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="238a4-138">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="238a4-139">`PostJsonAsync` &ndash; envía una solicitud HTTP POST, incluido el contenido con codificación JSON, y analiza el cuerpo de respuesta JSON para crear un objeto.</span><span class="sxs-lookup"><span data-stu-id="238a4-139">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="238a4-140">En el código siguiente, un elemento enlazado del componente proporciona `_newItemName`.</span><span class="sxs-lookup"><span data-stu-id="238a4-140">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="238a4-141">El método `AddItem` se desencadena seleccionando un elemento `<button>`.</span><span class="sxs-lookup"><span data-stu-id="238a4-141">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="238a4-142">Consulte la aplicación de ejemplo para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="238a4-142">See the sample app for a complete example.</span></span>

  ```razor
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

* <span data-ttu-id="238a4-143">`PutJsonAsync` &ndash; envía una solicitud HTTP PUT, incluido el contenido con codificación JSON.</span><span class="sxs-lookup"><span data-stu-id="238a4-143">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="238a4-144">En el código siguiente, los elementos enlazados del componente proporcionan `_editItem` valores para `Name` y `IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="238a4-144">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="238a4-145">El `Id` del elemento se establece cuando el elemento está seleccionado en otra parte de la interfaz de usuario y se llama a `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="238a4-145">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="238a4-146">El método `SaveItem` se desencadena seleccionando el elemento guardar `<button>`.</span><span class="sxs-lookup"><span data-stu-id="238a4-146">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="238a4-147">Consulte la aplicación de ejemplo para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="238a4-147">See the sample app for a complete example.</span></span>

  ```razor
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

<span data-ttu-id="238a4-148"><xref:System.Net.Http> incluye métodos de extensión adicionales para enviar solicitudes HTTP y recibir respuestas HTTP.</span><span class="sxs-lookup"><span data-stu-id="238a4-148"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="238a4-149">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) se usa para enviar una solicitud HTTP DELETE a una API Web.</span><span class="sxs-lookup"><span data-stu-id="238a4-149">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="238a4-150">En el código siguiente, el elemento Delete `<button>` llama al método `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="238a4-150">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="238a4-151">El elemento de `<input>` enlazado proporciona la `id` del elemento que se va a eliminar.</span><span class="sxs-lookup"><span data-stu-id="238a4-151">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="238a4-152">Consulte la aplicación de ejemplo para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="238a4-152">See the sample app for a complete example.</span></span>

```razor
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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="238a4-153">Uso compartido de recursos entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="238a4-153">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="238a4-154">La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que sirvió a la Página Web.</span><span class="sxs-lookup"><span data-stu-id="238a4-154">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="238a4-155">Esta restricción se llama *Directiva del mismo origen*.</span><span class="sxs-lookup"><span data-stu-id="238a4-155">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="238a4-156">La Directiva del mismo origen impide que un sitio malintencionado Lea datos confidenciales de otro sitio.</span><span class="sxs-lookup"><span data-stu-id="238a4-156">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="238a4-157">Para realizar solicitudes desde el explorador a un punto de conexión con un origen diferente, el *punto de conexión* debe habilitar el [uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="238a4-157">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="238a4-158">En la [Blazor aplicación de ejemplo Webassembly (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) se muestra el uso de CORS en el componente Call Web API (*pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="238a4-158">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="238a4-159">Para permitir que otros sitios realicen solicitudes de uso compartido de recursos entre orígenes (CORS) a la aplicación, consulte <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="238a4-159">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="238a4-160">HttpClient y HttpRequestMessage con las opciones de solicitud de la API fetch</span><span class="sxs-lookup"><span data-stu-id="238a4-160">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="238a4-161">Al ejecutar webassembly en una aplicación Blazor webassembly, use [HttpClient](xref:fundamentals/http-requests) y <xref:System.Net.Http.HttpRequestMessage> para personalizar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="238a4-161">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="238a4-162">Por ejemplo, puede especificar el URI de solicitud, el método HTTP y cualquier encabezado de solicitud deseado.</span><span class="sxs-lookup"><span data-stu-id="238a4-162">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

```razor
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

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="238a4-163">Para obtener más información sobre las opciones de la API de captura, vea [MDN web docs: WindowOrWorkerGlobalScope. fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="238a4-163">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="238a4-164">Al enviar credenciales (cookies de autorización/encabezados) en solicitudes de CORS, la Directiva de CORS debe permitir el encabezado `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="238a4-164">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="238a4-165">La siguiente directiva incluye la configuración de:</span><span class="sxs-lookup"><span data-stu-id="238a4-165">The following policy includes configuration for:</span></span>

* <span data-ttu-id="238a4-166">Orígenes de la solicitud (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="238a4-166">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="238a4-167">Cualquier método (verbo).</span><span class="sxs-lookup"><span data-stu-id="238a4-167">Any method (verb).</span></span>
* <span data-ttu-id="238a4-168">encabezados de `Content-Type` y `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="238a4-168">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="238a4-169">Para permitir un encabezado personalizado (por ejemplo, `x-custom-header`), enumere el encabezado al llamar a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="238a4-169">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="238a4-170">Credenciales establecidas por el código JavaScript del lado cliente (`credentials` propiedad establecida en `include`).</span><span class="sxs-lookup"><span data-stu-id="238a4-170">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="238a4-171">Para obtener más información, vea <xref:security/cors> y el componente del evaluador de solicitudes HTTP de la aplicación de ejemplo (*Components/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="238a4-171">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="238a4-172">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="238a4-172">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="238a4-173">Configuración del punto de conexión HTTPS de Kestrel</span><span class="sxs-lookup"><span data-stu-id="238a4-173">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="238a4-174">Uso compartido de recursos entre orígenes (CORS) en W3C</span><span class="sxs-lookup"><span data-stu-id="238a4-174">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
