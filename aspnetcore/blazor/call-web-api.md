---
title: Llamada a una API Web desde ASP.NET Core extraordinarias
author: guardrex
description: Obtenga información sobre cómo llamar a una API Web desde una aplicación increíblemente con aplicaciones auxiliares de JSON, incluida la realización de solicitudes de uso compartido de recursos entre orígenes (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/call-web-api
ms.openlocfilehash: 60ebd01bc07da22cd1dcd0b16297ee54c97867fc
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030390"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>Llamada a una API Web desde ASP.NET Core extraordinarias

Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)

Las aplicaciones de cliente extraordinarias llaman a las API Web mediante un `HttpClient` servicio preconfigurado. Cree solicitudes, que pueden incluir opciones de [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) de JavaScript, con aplicaciones auxiliares JSON más increíbles <xref:System.Net.Http.HttpRequestMessage>o con.

Las aplicaciones de servidor increíbles llaman a las API Web <xref:System.Net.Http.HttpClient> mediante instancias creadas normalmente <xref:System.Net.Http.IHttpClientFactory>mediante. Para obtener más información, consulta <xref:fundamentals/http-requests>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Para ver ejemplos de cliente más increíbles, consulte los siguientes componentes en la aplicación de ejemplo:

* Llamar a la API Web (*pages/CallWebAPI. Razor*)
* Evaluador de solicitudes HTTP (*Components/HTTPRequestTester. Razor*)

## <a name="httpclient-and-json-helpers"></a>Aplicaciones auxiliares de HttpClient y JSON

En el caso de las aplicaciones del lado cliente, [HttpClient](xref:fundamentals/http-requests) está disponible como un servicio preconfigurado para realizar solicitudes de vuelta al servidor de origen. Para usar `HttpClient` aplicaciones auxiliares de JSON, agregue una referencia de `Microsoft.AspNetCore.Blazor.HttpClient`paquete a. `HttpClient`las aplicaciones auxiliares de JSON también se usan para llamar a puntos de conexión de API Web de terceros. `HttpClient`se implementa mediante la [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) del explorador y está sujeto a sus limitaciones, incluido el cumplimiento de la misma directiva de origen.

La dirección base del cliente se establece en la dirección del servidor de origen. Inserte una `HttpClient` instancia mediante la `@inject` Directiva:

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

En los ejemplos siguientes, una API Web de todo procesa operaciones de creación, lectura, actualización y eliminación (CRUD). Los ejemplos se basan en una `TodoItem` clase que almacena:

* Identificador (`Id`, `long`) &ndash; identificador único del elemento.
* Nombre (`Name`, `string`) &ndash; nombre del elemento.
* Indica el`IsComplete`estado `bool`( &ndash; ,) si el elemento todo ha finalizado.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

Los métodos auxiliares de JSON envían solicitudes a un URI (una API Web en los ejemplos siguientes) y procesan la respuesta:

* `GetJsonAsync`&ndash; Envía una solicitud HTTP GET y analiza el cuerpo de respuesta JSON para crear un objeto.

  En el código siguiente, `_todoItems` se muestran en el componente. El `GetTodoItems` método se desencadena cuando finaliza la representación del componente ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)). Consulte la aplicación de ejemplo para obtener un ejemplo completo.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* `PostJsonAsync`&ndash; Envía una solicitud HTTP POST, incluido el contenido con codificación JSON, y analiza el cuerpo de respuesta JSON para crear un objeto.

  En el código siguiente, `_newItemName` lo proporciona un elemento enlazado del componente. El `AddItem` método se desencadena seleccionando un `<button>` elemento. Consulte la aplicación de ejemplo para obtener un ejemplo completo.

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
          await Http.PostJsonAsync("api/todo", addItem);
      }
  }
  ```

* `PutJsonAsync`&ndash; Envía una solicitud HTTP PUT, incluido el contenido con codificación JSON.

  En el código siguiente, `_editItem` los elementos `Name` enlazados del componente proporcionan los valores de y `IsCompleted` . La del `Id` elemento se establece cuando el elemento se selecciona en otra parte de la interfaz de `EditItem` usuario y se llama a. El `SaveItem` método se desencadena seleccionando el elemento Save `<button>` . Consulte la aplicación de ejemplo para obtener un ejemplo completo.

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
          await Http.PutJsonAsync($"api/todo/{_editItem.Id}, _editItem);
  }
  ```

<xref:System.Net.Http>incluye métodos de extensión adicionales para enviar solicitudes HTTP y recibir respuestas HTTP. [HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) se usa para enviar una solicitud HTTP DELETE a una API Web.

En el código siguiente, el elemento `<button>` Delete llama al `DeleteItem` método. El elemento `<input>` enlazado proporciona `id` la del elemento que se va a eliminar. Consulte la aplicación de ejemplo para obtener un ejemplo completo.

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/todo/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a>Uso compartido de recursos entre orígenes (CORS)

La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que sirvió a la Página Web. Esta restricción se llama *Directiva del mismo origen*. La Directiva del mismo origen impide que un sitio malintencionado Lea datos confidenciales de otro sitio. Para realizar solicitudes desde el explorador a un punto de conexión con un origen diferente, el *punto de conexión* debe habilitar el [uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/).

En la aplicación de ejemplo se muestra el uso de CORS en el componente Call Web API (*pages/CallWebAPI. Razor*).

Para permitir que otros sitios realicen solicitudes de uso compartido de recursos entre orígenes (CORS) a la <xref:security/cors>aplicación, vea.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient y HttpRequestMessage con las opciones de solicitud de la API fetch

Cuando se ejecuta en WebAssembly en una aplicación de cliente más increíble, use [HttpClient](xref:fundamentals/http-requests) y <xref:System.Net.Http.HttpRequestMessage> para personalizar las solicitudes. Por ejemplo, puede especificar el URI de solicitud, el método HTTP y cualquier encabezado de solicitud deseado.

Proporcione opciones de solicitud a la [API de captura](https://developer.mozilla.org/docs/Web/API/Fetch_API) de JavaScript `WebAssemblyHttpMessageHandler.FetchArgs` subyacente mediante la propiedad en la solicitud. Como se muestra en el ejemplo siguiente, `credentials` la propiedad se establece en cualquiera de los siguientes valores:

* `FetchCredentialsOption.Include`("include") &ndash; Aconseja al explorador que envíe las credenciales (como cookies o encabezados de autenticación http) incluso para las solicitudes entre orígenes. Solo se permite cuando la Directiva CORS está configurada para permitir credenciales.
* `FetchCredentialsOption.Omit`("omitir") &ndash; Informa al explorador de que nunca debe enviar credenciales (como cookies o encabezados de autenticación http).
* `FetchCredentialsOption.SameOrigin`("mismo origen") &ndash; Aconseja al explorador que envíe las credenciales (como cookies o encabezados de autenticación http) solo si la dirección URL de destino está en el mismo origen que la aplicación que realiza la llamada.

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
            RequestUri = new Uri("https://localhost:10000/api/todo"),
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

Para obtener más información sobre las opciones de la [API de Fetch, consulte documentos web de MDN: WindowOrWorkerGlobalScope. fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

Al enviar credenciales (cookies de autorización/encabezados) en solicitudes de `Authorization` CORS, la Directiva de CORS debe permitir el encabezado.

La siguiente directiva incluye la configuración de:

* Orígenes de la`http://localhost:5000`solicitud `https://localhost:5001`(,).
* Cualquier método (verbo).
* `Content-Type`encabezados `Authorization` y. Para permitir un encabezado personalizado (por ejemplo, `x-custom-header`), enumere el encabezado al llamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>a.
* Credenciales establecidas por el código JavaScript del lado`credentials` cliente (propiedad `include`establecida en).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Para obtener más información, <xref:security/cors> vea y el componente del evaluador de solicitudes HTTP de la aplicación de ejemplo (*Components/HTTPRequestTester. Razor*).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/http-requests>
* [Uso compartido de recursos entre orígenes (CORS) en W3C](https://www.w3.org/TR/cors/)
