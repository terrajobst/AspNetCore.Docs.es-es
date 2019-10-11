---
title: Llamada a una API Web desde ASP.NET Core extraordinarias
author: guardrex
description: Obtenga información sobre cómo llamar a una API Web desde una aplicación increíblemente con aplicaciones auxiliares de JSON, incluida la realización de solicitudes de uso compartido de recursos entre orígenes (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: blazor/call-web-api
ms.openlocfilehash: 3d70af2226eb29870458a5fd3c2bbbc3ee5c14ce
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037436"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>Llamada a una API Web desde ASP.NET Core extraordinarias

Por [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)y [Juan de la Cruz](https://github.com/juandelacruz23)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Las aplicaciones de webassembly increíbles llaman a las API Web mediante un servicio @no__t preconfigurado-0. Cree solicitudes, que pueden incluir opciones de [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) de JavaScript, con aplicaciones auxiliares JSON más increíbles o con <xref:System.Net.Http.HttpRequestMessage>.

Las aplicaciones de servidor increíbles llaman a las API Web mediante instancias de <xref:System.Net.Http.HttpClient> creadas normalmente con <xref:System.Net.Http.IHttpClientFactory>. Para obtener más información, consulta <xref:fundamentals/http-requests>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Para obtener ejemplos de webassembly más increíbles, vea los siguientes componentes en la aplicación de ejemplo:

* Llamar a la API Web (*pages/CallWebAPI. Razor*)
* Evaluador de solicitudes HTTP (*Components/HTTPRequestTester. Razor*)

## <a name="httpclient-and-json-helpers"></a>Aplicaciones auxiliares de HttpClient y JSON

En las aplicaciones de webassembly increíbles, [HttpClient](xref:fundamentals/http-requests) está disponible como un servicio preconfigurado para realizar solicitudes de vuelta al servidor de origen. Para usar las aplicaciones auxiliares JSON `HttpClient`, agregue una referencia de paquete a `Microsoft.AspNetCore.Blazor.HttpClient`. las aplicaciones auxiliares `HttpClient` y JSON también se usan para llamar a puntos de conexión de API Web de terceros. `HttpClient` se implementa mediante la [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) del explorador y está sujeto a sus limitaciones, incluido el cumplimiento de la misma directiva de origen.

La dirección base del cliente se establece en la dirección del servidor de origen. Inserte una instancia de `HttpClient` mediante la Directiva `@inject`:

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

En los ejemplos siguientes, una API Web de todo procesa operaciones de creación, lectura, actualización y eliminación (CRUD). Los ejemplos se basan en una clase `TodoItem` que almacena:

* ID. (`Id`, `long`) &ndash; ID. único del elemento.
* Nombre (`Name`, `string`) &ndash; nombre del elemento.
* Estado (`IsComplete`, `bool`) &ndash; Si el elemento todo ha finalizado.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

Los métodos auxiliares de JSON envían solicitudes a un URI (una API Web en los ejemplos siguientes) y procesan la respuesta:

* `GetJsonAsync` &ndash; envía una solicitud HTTP GET y analiza el cuerpo de respuesta JSON para crear un objeto.

  En el código siguiente, el componente muestra el `_todoItems`. El método `GetTodoItems` se desencadena cuando finaliza la representación del componente ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)). Consulte la aplicación de ejemplo para obtener un ejemplo completo.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* `PostJsonAsync` &ndash; envía una solicitud HTTP POST, incluido el contenido con codificación JSON, y analiza el cuerpo de respuesta JSON para crear un objeto.

  En el código siguiente, se proporciona `_newItemName` mediante un elemento enlazado del componente. El método `AddItem` se desencadena seleccionando un elemento `<button>`. Consulte la aplicación de ejemplo para obtener un ejemplo completo.

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

* `PutJsonAsync` &ndash; envía una solicitud HTTP PUT, incluido el contenido con codificación JSON.

  En el código siguiente, los elementos enlazados del componente proporcionan valores `_editItem` para `Name` y `IsCompleted`. El @no__t del elemento se establece cuando el elemento está seleccionado en otra parte de la interfaz de usuario y se llama a `EditItem`. El método `SaveItem` se desencadena seleccionando el elemento Save `<button>`. Consulte la aplicación de ejemplo para obtener un ejemplo completo.

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

<xref:System.Net.Http> incluye métodos de extensión adicionales para enviar solicitudes HTTP y recibir respuestas HTTP. [HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) se usa para enviar una solicitud HTTP DELETE a una API Web.

En el código siguiente, el elemento Delete `<button>` llama al método `DeleteItem`. El elemento `<input>` enlazado proporciona el `id` del elemento que se va a eliminar. Consulte la aplicación de ejemplo para obtener un ejemplo completo.

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

## <a name="cross-origin-resource-sharing-cors"></a>Uso compartido de recursos entre orígenes (CORS)

La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que sirvió a la Página Web. Esta restricción se llama *Directiva del mismo origen*. La Directiva del mismo origen impide que un sitio malintencionado Lea datos confidenciales de otro sitio. Para realizar solicitudes desde el explorador a un punto de conexión con un origen diferente, el *punto de conexión* debe habilitar el [uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/).

En la aplicación de ejemplo se muestra el uso de CORS en el componente Call Web API (*pages/CallWebAPI. Razor*).

Para permitir que otros sitios realicen solicitudes de uso compartido de recursos entre orígenes (CORS) a la aplicación, consulte <xref:security/cors>.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient y HttpRequestMessage con las opciones de solicitud de la API fetch

Cuando se ejecute en webassembly en una aplicación de webassembly increíblemente alta, use [HttpClient](xref:fundamentals/http-requests) y <xref:System.Net.Http.HttpRequestMessage> para personalizar las solicitudes. Por ejemplo, puede especificar el URI de solicitud, el método HTTP y cualquier encabezado de solicitud deseado.

Proporcione opciones de solicitud a la [API de captura](https://developer.mozilla.org/docs/Web/API/Fetch_API) de JavaScript subyacente mediante la propiedad `WebAssemblyHttpMessageHandler.FetchArgs` en la solicitud. Como se muestra en el ejemplo siguiente, la propiedad `credentials` se establece en cualquiera de los siguientes valores:

* `FetchCredentialsOption.Include` ("include") &ndash; aconseja al explorador que envíe las credenciales (como cookies o encabezados de autenticación HTTP) incluso para las solicitudes entre orígenes. Solo se permite cuando la Directiva CORS está configurada para permitir credenciales.
* `FetchCredentialsOption.Omit` ("omitir") &ndash; informa al explorador de que nunca debe enviar credenciales (como cookies o encabezados de autenticación HTTP).
* `FetchCredentialsOption.SameOrigin` ("Same-Origin") &ndash; aconseja al explorador que envíe las credenciales (como cookies o encabezados de autenticación HTTP) solo si la dirección URL de destino está en el mismo origen que la aplicación que realiza la llamada.

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

Para obtener más información sobre las opciones de la API de captura, vea [MDN web docs: WindowOrWorkerGlobalScope. fetch ():P arameters @ no__t-0.

Al enviar credenciales (cookies de autorización/encabezados) en solicitudes de CORS, la Directiva de CORS debe permitir el encabezado `Authorization`.

La siguiente directiva incluye la configuración de:

* Orígenes de la solicitud (`http://localhost:5000`, `https://localhost:5001`).
* Cualquier método (verbo).
* `Content-Type` y `Authorization` encabezados. Para permitir un encabezado personalizado (por ejemplo, `x-custom-header`), enumere el encabezado al llamar a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.
* Credenciales establecidas por el código JavaScript del lado cliente (propiedad `credentials` establecida en `include`).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Para obtener más información, vea <xref:security/cors> y el componente del evaluador de solicitudes HTTP de la aplicación de ejemplo (*Components/HTTPRequestTester. Razor*).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [Configuración del punto de conexión HTTPS de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Uso compartido de recursos entre orígenes (CORS) en W3C](https://www.w3.org/TR/cors/)
