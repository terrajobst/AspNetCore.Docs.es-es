---
title: Llamada a una API web desde Blazor de ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo llamar a una API web desde una aplicación Blazor mediante asistentes de JSON, incluida la realización de solicitudes de uso compartido de recursos entre orígenes (CORS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: e6996f0e6731b05038d0a9329152b8afd5f6796d
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78647735"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a>Llamada a una API web desde Blazor de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27) y [Juan De la Cruz](https://github.com/juandelacruz23)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Las aplicaciones [WebAssembly de Blazor](xref:blazor/hosting-models#blazor-webassembly) llaman a las API web mediante un servicio `HttpClient` preconfigurado. Redacte las solicitudes, que pueden incluir opciones [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) de JavaScript, mediante asistentes de JSON de Blazor o con <xref:System.Net.Http.HttpRequestMessage>. El servicio `HttpClient` en las aplicaciones WebAssembly de Blazor se centra en realizar solicitudes de vuelta al servidor de origen. Las instrucciones de este tema solo se aplican a las aplicaciones WebAssembly de Blazor.

Las aplicaciones de [servidor Blazor](xref:blazor/hosting-models#blazor-server) llaman a las API web mediante instancias de <xref:System.Net.Http.HttpClient>, creadas normalmente con <xref:System.Net.Http.IHttpClientFactory>. Las instrucciones de este tema no se aplican a las aplicaciones de servidor Blazor. Al desarrollar aplicaciones de servidor Blazor, siga las instrucciones de <xref:fundamentals/http-requests>.

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargar](xref:index#how-to-download-a-sample))&ndash; seleccione la aplicación *BlazorWebAssemblySample*.

Vea los componentes siguientes en la aplicación de ejemplo *BlazorWebAssemblySample*:

* Llamada a la API web (*Pages/CallWebAPI.razor*)
* Evaluador de solicitudes HTTP (*Components/HTTPRequestTester.razor*)

## <a name="packages"></a>Paquetes

Haga referencia al paquete NuGet *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) en el archivo del proyecto. `Microsoft.AspNetCore.Blazor.HttpClient` se basa en `HttpClient` y [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).

Para utilizar una API estable, use el paquete [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/), que usa [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm). El uso de la API estable en `Microsoft.AspNet.WebApi.Client` no proporciona los asistentes de JSON que se describen en este tema, que son únicos del paquete experimental `Microsoft.AspNetCore.Blazor.HttpClient`.

## <a name="httpclient-and-json-helpers"></a>HttpClient y asistentes de JSON

En una aplicación WebAssembly de Blazor, [HttpClient](xref:fundamentals/http-requests) está disponible como un servicio preconfigurado para realizar solicitudes de vuelta al servidor de origen.

De forma predeterminada, una aplicación de servidor Blazor no incluye un servicio `HttpClient`. Proporcione un servicio `HttpClient` a la aplicación mediante la [infraestructura de fábrica de HttpClient](xref:fundamentals/http-requests).

`HttpClient` y los asistentes de JSON también se usan para llamar a puntos de conexión de API web de terceros. `HttpClient` se implementa mediante [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) del explorador y está sujeto a sus limitaciones, incluido el cumplimiento de la directiva del mismo origen.

La dirección base del cliente se establece en la dirección del servidor de origen. Inserte una instancia de `HttpClient` mediante la directiva `@inject`:

```razor
@using System.Net.Http
@inject HttpClient Http
```

En los ejemplos siguientes, una API web Todo procesa operaciones de creación, lectura, actualización y eliminación (CRUD). Los ejemplos se basan en una clase `TodoItem` que almacena lo siguiente:

* Id. (`Id`, `long`): identificador único del elemento.
* Nombre (`Name`, `string`): nombre del elemento.
* Estado (`IsComplete`, `bool`): indicación de si el elemento Todo ha finalizado.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

Los métodos auxiliares de JSON envían solicitudes a un URI (una API web en los ejemplos siguientes) y procesan la respuesta:

* `GetJsonAsync`: envía una solicitud HTTP GET y analiza el cuerpo de respuesta JSON para crear un objeto.

  En el código siguiente, el componente muestra el elemento `_todoItems`. El método `GetTodoItems` se desencadena cuando finaliza la representación del componente ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)). Vea la aplicación de ejemplo para obtener un ejemplo completo.

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* `PostJsonAsync`: envía una solicitud HTTP POST, incluido el contenido con codificación JSON, y analiza el cuerpo de respuesta JSON para crear un objeto.

  En el código siguiente, un elemento enlazado del componente proporciona `_newItemName`. El método `AddItem` se desencadena al seleccionar un elemento `<button>`. Vea la aplicación de ejemplo para obtener un ejemplo completo.

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

* `PutJsonAsync`: envía una solicitud HTTP PUT, incluido el contenido con codificación JSON.

  En el código siguiente, los elementos enlazados del componente proporcionan valores `_editItem` para `Name` e `IsCompleted`. El valor `Id` del elemento se establece cuando el elemento está seleccionado en otra parte de la interfaz de usuario y se llama a `EditItem`. El método `SaveItem` se desencadena al seleccionar un objeto `<button>` Guardar. Vea la aplicación de ejemplo para obtener un ejemplo completo.

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

<xref:System.Net.Http> incluye métodos de extensión adicionales para enviar solicitudes HTTP y recibir respuestas HTTP. [HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) se usa para enviar una solicitud HTTP DELETE a una API web.

En el código siguiente, el elemento `<button>` Delete llama al método `DeleteItem`. El elemento `<input>` enlazado proporciona el valor `id` del elemento que se va a eliminar. Vea la aplicación de ejemplo para obtener un ejemplo completo.

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

## <a name="cross-origin-resource-sharing-cors"></a>Uso compartido de recursos entre orígenes (CORS)

La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que ha proporcionado esa página web. Esta restricción se denomina *directiva de mismo origen*. La directiva de mismo origen evita que un sitio malintencionado lea información confidencial de otro sitio. Para realizar solicitudes desde el explorador a un punto de conexión con un origen diferente, el *punto de conexión* debe habilitar el [uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/).

En la [aplicación de ejemplo WebAssembly de Blazor (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) se muestra el uso de CORS en el componente Call Web API (*Pages/CallWebAPI.razor*).

Para permitir que otros sitios realicen solicitudes de uso compartido de recursos entre orígenes (CORS) a la aplicación, vea <xref:security/cors>.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient y HttpRequestMessage con las opciones de solicitud de Fetch API

Al ejecutar WebAssembly en una aplicación WebAssembly de Blazor, use [HttpClient](xref:fundamentals/http-requests) y <xref:System.Net.Http.HttpRequestMessage> para personalizar las solicitudes. Por ejemplo, puede especificar el URI de la solicitud, el método HTTP y cualquier encabezado de solicitud deseado.

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

Para obtener más información sobre las opciones de Fetch API, vea [Documentación web de MDN: WindowOrWorkerGlobalScope.fetch(): parámetros](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

Al enviar credenciales (cookies de autorización o encabezados) en solicitudes de CORS, la directiva de CORS debe permitir el encabezado `Authorization`.

En la directiva siguiente se incluye la configuración de:

* Orígenes de la solicitud (`http://localhost:5000`, `https://localhost:5001`).
* Cualquier método (verbo).
* Los encabezados `Content-Type` y `Authorization`. Para permitir un encabezado personalizado (por ejemplo, `x-custom-header`), enumere el encabezado al llamar a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.
* Las credenciales establecidas por el código JavaScript del lado cliente (la propiedad `credentials` establecida en `include`).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Para obtener más información, vea <xref:security/cors> y el componente del evaluador de solicitudes HTTP de la aplicación de ejemplo (*Components/HTTPRequestTester.razor*).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [Configuración de puntos de conexión HTTPS de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Uso compartido de recursos entre orígenes (CORS) en W3C](https://www.w3.org/TR/cors/)
