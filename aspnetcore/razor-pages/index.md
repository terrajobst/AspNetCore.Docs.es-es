---
title: Introducción a las páginas de Razor en ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo las páginas de Razor de ASP.NET Core facilitan la programación de escenarios centrados en páginas y hacen que resulte más productiva que con MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
ms.openlocfilehash: 601d6ac2cb373c40fb1de5427b0ea6c299fa1f32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296752"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>Introducción a las páginas de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Ryan Nowak](https://github.com/rynowak)

Las páginas de Razor son una nueva característica de ASP.NET Core MVC que facilita la codificación de escenarios centrados en páginas y hace que sea más productiva.

Si busca un tutorial que use el enfoque Model-View-Controller, consulte [Introducción a ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).

En este documento se proporciona una introducción a las páginas de Razor. No es un tutorial paso a paso. Si encuentra que alguna sección es demasiado avanzada, consulte [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start). Para obtener información general de ASP.NET Core, vea [Introducción a ASP.NET Core](xref:index).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a>Crear un proyecto de páginas de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Vea [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start) para obtener instrucciones detalladas sobre cómo crear un proyecto de páginas de Razor con Visual Studio.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

Ejecute `dotnet new webapp` desde la línea de comandos.

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Ejecute `dotnet new razor` desde la línea de comandos.

::: moniker-end

Abra el archivo *.csproj* generado desde Visual Studio para Mac.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

::: moniker range=">= aspnetcore-2.1"

Ejecute `dotnet new webapp` desde la línea de comandos.

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Ejecute `dotnet new razor` desde la línea de comandos.

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli) 

::: moniker range=">= aspnetcore-2.1"

Ejecute `dotnet new webapp` desde la línea de comandos.

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Ejecute `dotnet new razor` desde la línea de comandos.

::: moniker-end

---

## <a name="razor-pages"></a>Páginas de Razor

Páginas de Razor está habilitada en *Startup.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Considere la posibilidad de una página básica: <a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

El código anterior es muy parecido a un archivo de vista de Razor. La directiva `@page` lo hace diferente. `@page` transforma el archivo en una acción de MVC, lo que significa que administra las solicitudes directamente, sin tener que pasar a través de un controlador. `@page` debe ser la primera directiva de Razor de una página. `@page` afecta al comportamiento de otras construcciones de Razor.

Una página similar, con una clase `PageModel`, se muestra en los dos archivos siguientes. El archivo *Pages/Index2.cshtml*:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

Modelo de página *Pages/Index2.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Por convención, el archivo de clase `PageModel` tiene el mismo nombre que el archivo de páginas de Razor con *.cs* anexado. Por ejemplo, la página de Razor anterior es *Pages/Index2.cshtml*. El archivo que contiene la clase `PageModel` se denomina *Pages/Index2.cshtml.cs*.

Las asociaciones de rutas de dirección URL a páginas se determinan según la ubicación de la página en el sistema de archivos. En la tabla siguiente, se muestra una ruta de acceso de página de Razor y la dirección URL correspondiente:

| Ruta de acceso y nombre de archivo               | URL correspondiente |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` o `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` o `/Store/Index` |

Notas:

* El runtime busca archivos de páginas de Razor en la carpeta *Páginas* de forma predeterminada.
* `Index` es la página predeterminada cuando una URL no incluye una página.

## <a name="writing-a-basic-form"></a>Escribir un formulario básico

Las páginas de Razor están diseñadas para facilitar la implementación de patrones comunes que se usan con exploradores web al compilar una aplicación. Los [enlaces de modelos](xref:mvc/models/model-binding), las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) y las aplicaciones auxiliares de HTML *simplemente funcionan* con las propiedades definidas en una clase de página de Razor. Considere la posibilidad de una página que implementa un formulario básico del estilo "Póngase en contacto con nosotros" para el modelo `Contact`:

Para los ejemplos de este documento, `DbContext` se inicializa en el archivo [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

El modelo de datos:

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

El contexto de la base de datos:

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

El archivo de vista *Pages/Create.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

Modelo de página *Pages/Create.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Por convención, la clase `PageModel` se denomina `<PageName>Model` y se encuentra en el mismo espacio de nombres que la página.

La clase `PageModel` permite la separación de la lógica de una página de su presentación. Define los controladores de página para solicitudes que se envían a la página y los datos que usan para representar la página. Esta separación le permite administrar dependencias de páginas mediante la [inyección de dependencias](xref:fundamentals/dependency-injection) y para realizar [pruebas unitarias](xref:test/razor-pages-tests) de las páginas.

La página tiene un *método de controlador* `OnPostAsync`, que se ejecuta en solicitudes `POST` (cuando un usuario envía el formulario). Puede agregar métodos de controlador para cualquier verbo HTTP. Los controladores más comunes son:

* `OnGet` para inicializar el estado necesario para la página. Ejemplo [OnGet](#OnGet).
* `OnPost` para controlar los envíos del formulario.

El sufijo de nombre `Async` es opcional, pero se usa a menudo por convención para funciones asincrónicas. El código `OnPostAsync` en el ejemplo anterior es similar a lo que escribiría normalmente en un controlador. El código anterior es típico de las páginas de Razor. La mayoría de primitivos MVC como el [enlace de modelos](xref:mvc/models/model-binding), la [validación](xref:mvc/models/validation) y los resultados de acciones se comparten.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

El método `OnPostAsync` anterior:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

El flujo básico de `OnPostAsync`:

Compruebe los errores de validación.

*  Si no hay ningún error, guarde los datos y redirija.
*  Si hay errores, muestre la página de nuevo con mensajes de validación. La validación del lado cliente es idéntica a las aplicaciones de ASP.NET Core MVC tradicionales. En muchos casos, los errores de validación se detectan en el cliente y nunca se envían al servidor.

Cuando los datos se escriben correctamente, el método del controlador `OnPostAsync` llama al método auxiliar `RedirectToPage` para devolver una instancia de `RedirectToPageResult`. `RedirectToPage` es un resultado de acción nueva, similar a `RedirectToAction` o `RedirectToRoute`, pero personalizada para las páginas. En el ejemplo anterior, redirige a la página de índice raíz (`/Index`). `RedirectToPage` se detalla en la sección [Generación de direcciones URL para las páginas](#url_gen).

Cuando el formulario enviado tiene errores de validación (que se pasan al servidor), el método del controlador `OnPostAsync` llama al método auxiliar `Page`. `Page` devuelve una instancia de `PageResult`. Devolver `Page` es similar a cómo las acciones en los controladores devuelven `View`. `PageResult` es el tipo de valor devuelto <!-- Review  --> predeterminado para un método de controlador. Un método de controlador que devuelve `void` representa la página.

La propiedad `Customer` usa el atributo `[BindProperty]` para participar en el enlace de modelos.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

De forma predeterminada, las páginas de Razor enlazan propiedades solo con verbos que no sean GET. Enlazar a propiedades puede reducir la cantidad de código que se debe escribir. Enlazar reduce el código al usar la misma propiedad para representar los campos de formulario (`<input asp-for="Customer.Name" />`) y aceptar la entrada.

> [!NOTE]
> Por motivos de seguridad, debe participar en el enlace de datos de solicitud GET con las propiedades del modelo de página. Compruebe las entradas de los usuarios antes de asignarlas a las propiedades. Si participa en este comportamiento, le puede ser útil al trabajar con escenarios que dependan de cadenas de consultas o valores de rutas.
>
> Para enlazar una propiedad en solicitudes GET, establezca la propiedad `SupportsGet` del atributo `[BindProperty]` como `true`: `[BindProperty(SupportsGet = true)]`

La página principal (*Index.cshtml*):

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

La clase `PageModel` asociada (*Index.cshtml.cs*):

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

El archivo *Index.cshtml* contiene el siguiente marcado para crear un vínculo de edición para cada contacto:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

La [aplicación auxiliar de etiquetas delimitadoras](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) ha usado el atributo `asp-route-{value}` para generar un vínculo a la página de edición. El vínculo contiene datos de ruta con el identificador del contacto. Por ejemplo: `http://localhost:5000/Edit/1`.

El archivo *Pages/Edit.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

La primera línea contiene la directiva `@page "{id:int}"`. La restricción de enrutamiento `"{id:int}"` indica a la página que acepte las solicitudes a la página que contienen datos de ruta `int`. Si una solicitud a la página no contiene datos de ruta que se puedan convertir en `int`, el tiempo de ejecución devuelve un error HTTP 404 (no encontrado). Para que el identificador sea opcional, anexe `?` a la restricción de ruta:

 ```cshtml
@page "{id:int?}"
```

El archivo *Pages/Edit.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

El archivo *index.cshtml* también contiene una marca para crear un botón de eliminar para cada contacto de cliente:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Al representar dicho botón de eliminar en HTML, `formaction` incluye parámetros para:

* Id. de contacto de cliente especificado mediante el atributo `asp-route-id`.
* `handler` especificado mediante el atributo `asp-page-handler`.

Aquí tiene un ejemplo de un botón de eliminar representado con un id. de contacto de cliente de `1`:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Al seleccionar el botón, se envía una solicitud de formulario `POST` al servidor. De forma predeterminada, el nombre del método de control se selecciona de acuerdo con el valor del parámetro `handler` y según el esquema `OnPost[handler]Async`.

Como en este ejemplo `handler` es `delete`, el método de control `OnPostDeleteAsync` se usa para procesar la solicitud `POST`. Si `asp-page-handler` se establece en otro valor, como `remove`, se seleccionará un método de control de páginas con el nombre `OnPostRemoveAsync`.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

El método `OnPostDeleteAsync` realiza las acciones siguientes:

* Acepta el elemento `id` de la cadena de consulta.
* Realiza una consulta a la base de datos del contacto de cliente con `FindAsync`.
* Si encuentra dicho contacto de cliente, se quita de la lista correspondiente. Luego, se actualiza la base de datos.
* Llama a `RedirectToPage` para redirigir la página Index raíz (`/Index`).

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a>Es necesario marcar las propiedades de página

Las propiedades de un valor `PageModel` se pueden decorar con el atributo [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute):

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

Para más información, vea [Validación de modelos](xref:mvc/models/validation).

## <a name="manage-head-requests-with-the-onget-handler"></a>Administración de solicitudes HEAD con el controlador OnGet

Normalmente, se crea un controlador HEAD al que se llama para las solicitudes HEAD:

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

Si no se define ningún controlador HEAD (`OnHead`), las páginas de Razor vuelven a llamar al controlador de páginas GET (`OnGet`) en ASP.NET Core 2.1 o posterior. Tiene la opción de usar este comportamiento con el [método SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) en `Startup.Configure` para ASP.NET Core 2.1 a 2.x:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

`SetCompatibilityVersion` define con eficacia la opción de las páginas de Razor `AllowMappingHeadRequestsToGetHandler` como `true`.

En lugar de participar en todos los comportamientos de 2.1 con `SetCompatibilityVersion`, puede participar explícitamente en comportamientos específicos. El código que se indica a continuación participa en las solicitudes HEAD de asignación del controlador GET.


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF y páginas de Razor

No tiene que escribir ningún código para la [validación antifalsificación](xref:security/anti-request-forgery). La validación y generación de tokens antifalsificación se incluyen automáticamente en las páginas de Razor.

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Usar diseños, parciales, plantillas y aplicaciones auxiliares de etiquetas con las páginas de Razor

Las páginas funcionan con todas las características del motor de vista de Razor. Los diseños, parciales, plantillas, aplicaciones auxiliares de etiquetas, *_ViewStart.cshtml*, *_ViewImports.cshtml* funcionan de la misma manera que lo hacen con las vistas de Razor convencionales.

Para simplificar esta página, aprovecharemos algunas de esas características.

Agregue una [página de diseño](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

El [diseño](xref:mvc/views/layout):

* Controla el diseño de cada página (a no ser que la página no tenga diseño).
* Importa las estructuras HTML como JavaScript y hojas de estilos.

Vea [Layout page](xref:mvc/views/layout) (Página de diseño) para obtener más información.

La propiedad [Layout](xref:mvc/views/layout#specifying-a-layout) se establece en *Pages/_ViewStart.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

El diseño está en la carpeta *Pages*. Las páginas buscan otras vistas (diseños, plantillas, parciales) de forma jerárquica, a partir de la misma carpeta que la página actual. Un diseño en la carpeta *Pages* se puede usar desde cualquier página de Razor en la carpeta *Pages*.

Le recomendamos que **no** coloque el archivo de diseño en la carpeta *Views/Shared*. *Views/Shared* es un patrón de vistas de MVC. Las páginas de Razor están diseñadas para basarse en la jerarquía de carpetas, no en las convenciones de ruta de acceso.

La búsqueda de vistas de una página de Razor incluye la carpeta *Pages*. Los diseños, plantillas y parciales que usa con los controladores de MVC y las vistas de Razor convencionales *simplemente funcionan*.

Agregue un archivo *Pages/_ViewImports.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` se explica más adelante en el tutorial. La directiva `@addTagHelper` pone las [aplicaciones auxiliares de etiquetas integradas](xref:mvc/views/tag-helpers/builtin-th/Index) en todas las páginas de la carpeta *Pages*.

<a name="namespace"></a>

Cuando la directiva `@namespace` se usa explícitamente en una página:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

La directiva establece el espacio de nombres de la página. La directiva `@model` no necesita incluir el espacio de nombres.

Cuando la directiva `@namespace` se encuentra en *_ViewImports.cshtml*, el espacio de nombres especificado proporciona el prefijo del espacio de nombres generado en la página que importa la directiva `@namespace`. El resto del espacio de nombres generado (la parte del sufijo) es la ruta de acceso relativa separada por puntos entre la carpeta que contiene *_ViewImports.cshtml* y la carpeta que contiene la página.

Por ejemplo, la clase `PageModel` *Pages/Customers/Edit.cshtml.cs* establece explícitamente el espacio de nombres:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

El archivo *Pages/_ViewImports.cshtml* establece el espacio de nombres siguiente:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

El espacio de nombres generado para la página de Razor *Pages/Customers/Edit.cshtml* es el mismo que la clase `PageModel`.

`@namespace` *también funciona con las vistas de Razor convencionales*.

El archivo de vista *Pages/Create.cshtml* original:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

El archivo de vista *Pages/Create.cshtml* actualizado:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

El [proyecto de inicio de las páginas de Razor](#rpvs17) contiene *Pages/_ValidationScriptsPartial.cshtml*, que enlaza la validación del lado cliente.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Generación de direcciones URL para las páginas

La página `Create`, mostrada anteriormente, usa `RedirectToPage`:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

La aplicación tiene la siguiente estructura de archivos o carpetas:

* */Pages*

  * *Index.cshtml*
  * */Customers*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

Las páginas *Pages/Customers/Create.cshtml* y *Pages/Customers/Edit.cshtml* redirigen a *Pages/Index.cshtml* si se realiza correctamente. La cadena `/Index` forma parte del URI para tener acceso a la página anterior. La cadena `/Index` puede usarse para generar los URI para la página *Pages/Index.cshtml*. Por ejemplo:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

El nombre de página es la ruta de acceso a la página de la carpeta raíz */Pages*, incluido un `/` inicial, por ejemplo `/Index`. Los ejemplos anteriores de generación de URL ofrecen opciones mejoradas y capacidades funcionales en comparación con la escritura a mano de estas. La generación de direcciones URL usa el [enrutamiento](xref:mvc/controllers/routing) y puede generar y codificar parámetros según cómo se defina la ruta en la ruta de acceso de destino.

La generación de direcciones URL para las páginas admite nombres relativos. En la siguiente tabla, se muestra qué página de índice está seleccionada con diferentes parámetros `RedirectToPage` de *Pages/Customers/Create.cshtml*:

| RedirectToPage(x)| Página |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Pages/Index* |
| RedirectToPage("./Index"); | *Pages/Customers/Index* |
| RedirectToPage("../Index") | *Pages/Index* |
| RedirectToPage("Index")  | *Pages/Customers/Index* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")` y `RedirectToPage("../Index")` son <em>nombres relativos</em>. El parámetro `RedirectToPage` se <em>combina</em> con la ruta de acceso de la página actual para calcular el nombre de la página de destino.  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

Vincular el nombre relativo es útil al crear sitios con una estructura compleja. Si usa nombres relativos para vincular entre páginas en una carpeta, puede cambiar el nombre de esa carpeta. Todos los vínculos seguirán funcionando (porque no incluyen el nombre de carpeta).

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a>Atributo ViewData

Se pueden pasar datos a una página con [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute). Los valores de las propiedades de controladores o modelos de página de Razor decoradas con `[ViewData]` se almacenan y cargan desde [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).

En el ejemplo siguiente, el valor `AboutModel` contiene una propiedad `Title` decorada con `[ViewData]`. La propiedad `Title` se establece en el título de la página Acerca de:

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

En la página Acerca de, acceda a la propiedad `Title` como propiedad de modelo:

```cshtml
<h1>@Model.Title</h1>
```

En el diseño, el título se lee desde el diccionario ViewData:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core expone la propiedad [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) en un [controlador](/dotnet/api/microsoft.aspnetcore.mvc.controller). Esta propiedad almacena datos hasta que se leen. Los métodos `Keep` y `Peek` se pueden usar para examinar los datos sin que se eliminen. `TempData` es útil para el redireccionamiento cuando se necesitan los datos de más de una única solicitud.

El atributo `[TempData]` es nuevo en ASP.NET Core 2.0 y es compatible con controladores y páginas.

El siguiente código establece el valor de `Message` mediante `TempData`:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

El siguiente marcado en el archivo *Pages/Customers/Index.cshtml* muestra el valor de `Message` mediante `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

El modelo de página *Pages/Customers/Index.cshtml.cs* aplica el atributo `[TempData]` a la propiedad `Message`.

```cs
[TempData]
public string Message { get; set; }
```

Vea [TempData](xref:fundamentals/app-state#tempdata) para obtener más información.

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a>Varios controladores por página

La siguiente página genera marcado para dos controladores de páginas mediante la aplicación auxiliar de etiquetas `asp-page-handler`:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

El formulario del ejemplo anterior tiene dos botones de envío, y cada uno de ellos usa `FormActionTagHelper` para enviar a una dirección URL diferente. El atributo `asp-page-handler` es un complemento de `asp-page`. `asp-page-handler` genera direcciones URL que envían a cada uno de los métodos de controlador definidos por una página. `asp-page` no se especifica porque el ejemplo se vincula a la página actual.

Modelo de página:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

El código anterior usa *métodos de controlador con nombre*. Los métodos de controlador con nombre se crean tomando el texto en el nombre después de `On<HTTP Verb>` y antes de `Async` (si existe). En el ejemplo anterior, los métodos de página son OnPost**JoinList**Async y OnPost**JoinListUC**Async. Quitando *OnPost* y *Async*, los nombres de controlador son `JoinList` y `JoinListUC`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Al usar el código anterior, la ruta de dirección URL que envía a `OnPostJoinListAsync` es `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. La ruta de dirección URL que envía a `OnPostJoinListUCAsync` es `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.

## <a name="custom-routes"></a>Rutas personalizadas

Use la directiva `@page` para:

* Especificar una ruta personalizada a una página. Por ejemplo, la ruta a la página Acerca de se puede establecer en `/Some/Other/Path` con `@page "/Some/Other/Path"`.
* Anexar segmentos a la ruta predeterminada de una página. Por ejemplo, se puede agregar un segmento "item" a la ruta predeterminada de una página con `@page "item"`.
* Anexar parámetros a la ruta predeterminada de una página. Por ejemplo, un parámetro de identificador, `id`, puede ser necesario para una página con `@page "{id}"`.

Se admite una ruta de acceso relativa raíz designada por una tilde (`~`) al principio de la ruta de acceso. Por ejemplo, `@page "~/Some/Other/Path"` es lo mismo que `@page "/Some/Other/Path"`.

La cadena de consulta `?handler=JoinList` de la dirección URL se puede cambiar por un segmento de ruta `/JoinList`, para lo cual hay que especificar la plantilla de ruta `@page "{handler?}"`.

Si no le gusta la cadena de consulta `?handler=JoinList` en la dirección URL, puede cambiar la ruta para poner el nombre del controlador en la parte de la ruta de la dirección URL. Para personalizar la ruta, se puede agregar una plantilla de ruta entre comillas dobles después de la directiva `@page`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

Al usar el código anterior, la ruta de dirección URL que envía a `OnPostJoinListAsync` es `http://localhost:5000/Customers/CreateFATH/JoinList`. La ruta de dirección URL que envía a `OnPostJoinListUCAsync` es `http://localhost:5000/Customers/CreateFATH/JoinListUC`.

El signo `?` que sigue a `handler` significa que el parámetro de ruta es opcional.

## <a name="configuration-and-settings"></a>Valores de configuración

Para configurar opciones avanzadas, use el método de extensión `AddRazorPagesOptions` en el generador de MVC:

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Actualmente, puede usar `RazorPagesOptions` para establecer el directorio raíz de páginas, o agregar convenciones de modelo de aplicación a las páginas. En el futuro habilitaremos más extensibilidad de este modo.

Para precompilar vistas, vea [Razor view compilation](xref:mvc/views/view-compilation) (Compilación de vistas de Razor).

[Descargue o vea el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).

Vea [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start), que se basa en esta introducción.

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Especificar que las páginas de Razor se encuentran en la raíz de contenido

De forma predeterminada, la ruta raíz de las páginas de Razor es el directorio */Pages*. Agregue [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que las páginas de Razor están en la ruta raíz de contenido ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) de la aplicación:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Especificar que las páginas de Razor se encuentran en un directorio raíz personalizado

Agregue [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que las páginas de Razor están en un directorio raíz personalizado en la aplicación (proporcione una ruta de acceso relativa):

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a>Vea también

* [Introducción a ASP.NET Core](xref:index)
* [Sintaxis de Razor](xref:mvc/views/razor)
* [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Convenciones de autorización de las páginas de Razor](xref:security/authorization/razor-pages-authorization)
* [Proveedores personalizados de rutas y modelos de página de páginas de Razor](xref:razor-pages/razor-pages-conventions)
* [Pruebas unitarias de páginas de Razor](xref:test/razor-pages-tests)
