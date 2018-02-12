---
title: "Aplicación auxiliar de etiquetas delimitadoras en ASP.NET Core"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiquetas delimitadoras"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a>Aplicación auxiliar de etiquetas delimitadoras

Por [Peter Kellner](http://peterkellner.net) 

La aplicación auxiliar de etiquetas delimitadoras mejora la etiqueta delimitadora HTML (`<a ... ></a>`) mediante la adición de nuevos atributos. El vínculo generado (en la etiqueta `href`) se crea con los nuevos atributos. Esa dirección URL puede incluir un protocolo opcional como https.

El siguiente controlador de altavoz se utiliza en los ejemplos de este documento.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Atributos de aplicación auxiliar de etiquetas delimitadoras

### <a name="asp-controller"></a>asp-controller

`asp-controller` se utiliza para asociar el controlador que se usará para generar la dirección URL. Los controladores especificados deben existir en el proyecto actual. El código siguiente enumera todos los altavoces: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

El marcado generado será:

```html
<a href="/Speaker">All Speakers</a>
```

Si se especifica `asp-controller` pero no `asp-action`, el valor predeterminado `asp-action` será el método de controlador predeterminado de la vista que se está ejecutando en ese momento. Es decir, en el ejemplo anterior, si se omite `asp-action`, y esta aplicación auxiliar de etiquetas delimitadoras se genera desde la vista `Index` de *HomeController* (**/Home**), el marcado generado será:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp-action

`asp-action` es el nombre del método de acción en el controlador que se incluirá en el `href` generado. Por ejemplo, el código siguiente establece el `href` generado para que apunte a la página de detalles del altavoz:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

El marcado generado será:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Si no hay ningún atributo `asp-controller` especificado, se usará el controlador predeterminado que llama a la vista que se ejecuta actualmente.  
 
Si el atributo `asp-action` es `Index`, no se anexa ninguna acción a la dirección URL, por lo que se llama al método `Index` predeterminado. La acción especificada (o su valor predeterminado), debe existir en el controlador al que se hace referencia en `asp-controller`.

### <a name="asp-page"></a>asp-page

Use el atributo `asp-page` en una etiqueta delimitadora para establecer su dirección URL de modo que señale a una página específica. La dirección URL se crea al prefijar el nombre de la página con una barra diagonal "/". La dirección URL del ejemplo siguiente apunta a la página "Altavoz" en el directorio actual.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

El atributo `asp-page` en el ejemplo de código anterior representa una salida HTML en la vista similar al fragmento de código siguiente:

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

El atributo `asp-page` es mutuamente excluyente con los atributos `asp-route`, `asp-controller` y `asp-action`. Pero `asp-page` puede utilizarse con `asp-route-id` para controlar el enrutamiento, como se muestra en el ejemplo de código siguiente:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

`asp-route-id` genera el siguiente resultado:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Para usar el atributo `asp-page` en las páginas de Razor, las direcciones URL deben ser una ruta de acceso relativa, por ejemplo `"./Speaker"`. Las rutas de acceso relativas en el atributo `asp-page` no están disponibles en las vistas de MVC. En su lugar, use la sintaxis "/" para las vistas de MVC.

### <a name="asp-route-value"></a>asp-route-{valor}

`asp-route-` es un prefijo de ruta comodín. Cualquier valor que se coloque después del guion final se interpretará como un parámetro de ruta potencial. Si no se encuentra una ruta predeterminada, este prefijo de ruta se anexará a la etiqueta href generada como un parámetro de solicitud y un valor. En caso contrario, se sustituirá en la plantilla de ruta.

Suponiendo que tenga un método de controlador definido del modo siguiente:

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

Y que haya definido la plantilla de ruta predeterminada en su *Startup.cs* como se indica a continuación:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

A continuación se muestra el archivo **cshtml** que contiene la aplicación auxiliar de etiquetas delimitadoras necesaria para utilizar el parámetro de modelo **speaker** que se pasa desde el controlador a la vista:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

El código HTML generado será como sigue, dado que **id** se encontró en la ruta predeterminada.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Si el prefijo de ruta no forma parte de la plantilla de enrutamiento encontrada, como es el caso del siguiente archivo **cshtml**:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

El código HTML generado será como sigue, dado que **speakerid** no se encontró en la ruta coincidente:

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Si no se especifica `asp-controller` o `asp-action`, se sigue el mismo proceso predeterminado que en el atributo `asp-route`.

### <a name="asp-route"></a>asp-route

`asp-route` proporciona una manera de crear una dirección URL que se vincula directamente a una ruta con nombre. Utilizando atributos de enrutamiento, puede asignarse el nombre a una ruta tal y como se muestra en `SpeakerController` y usarse en su método `Evaluations`.

`Name = "speakerevals"` indica a la aplicación auxiliar de etiquetas delimitadoras que genere una ruta directa a ese método de controlador usando la dirección URL `/Speaker/Evaluations`. Si además de `asp-route` se especifica `asp-controller` o `asp-action`, la ruta generada puede no ser la esperada. `asp-route` no debe usarse con ninguno de los atributos `asp-controller` o `asp-action` para evitar un conflicto de ruta.

### <a name="asp-all-route-data"></a>asp-all-route-data

`asp-all-route-data` permite crear un diccionario de pares clave/valor donde la clave es el nombre del parámetro y el valor es el valor asociado a esa clave.

Como se muestra en el ejemplo siguiente, se crea un diccionario insertado y los datos se pasan a la vista de Razor. También hay la opción de pasar los datos con su modelo.

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

El código anterior genera la siguiente dirección URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

Cuando se hace clic en el vínculo, se llama al método de controlador `EvaluationsCurrent`. La llamada se realiza porque ese controlador tiene dos parámetros de cadena que coinciden con lo que se ha creado a partir del diccionario `asp-all-route-data`.

Si alguna de las claves del diccionario coincide con los parámetros de ruta, se sustituirán los valores en la ruta según corresponda y los demás valores no coincidentes se generarán como parámetros de solicitud.

### <a name="asp-fragment"></a>asp-fragment

`asp-fragment` define un fragmento de dirección URL que se anexará a la dirección URL. La aplicación auxiliar de etiquetas delimitadoras agregará el carácter de almohadilla (#). Si crea una etiqueta:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

La dirección URL generada será: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Las etiquetas hash son útiles al crear aplicaciones del lado cliente. Por ejemplo, se pueden usar para el marcado y la búsqueda en JavaScript.

### <a name="asp-area"></a>asp-area

`asp-area` establece el nombre del área que ASP.NET Core utiliza para establecer la ruta adecuada. A continuación se muestran ejemplos de cómo el atributo de área hace una reasignación de rutas. Al establecer `asp-area` en Blogs, el directorio `Areas/Blogs` se prefija en las rutas de los controladores y vistas asociados de esta etiqueta delimitadora.

* Nombre de proyecto
  * wwwroot
  * Áreas
    * Blogs
      * Controladores
        * HomeController.cs
      * Vistas
        * Página principal
          * Index.cshtml
          * AboutBlog.cshtml
  * Controladores

Especificar una etiqueta de área que sea válida, como ```area="Blogs"``` al hacer referencia al archivo ```AboutBlog.cshtml```, tendrá un aspecto similar al siguiente mediante la aplicación auxiliar de etiquetas delimitadoras.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

El código HTML generado incluirá el segmento de áreas y será así:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Para que las áreas MVC funcionen en una aplicación web, la plantilla de ruta debe incluir una referencia al área si existe. Esa plantilla, que es el segundo parámetro de la llamada al método `routes.MapRoute`, se mostrará como: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp-protocol

`asp-protocol` sirve para especificar un protocolo (como `https`) en la dirección URL. El siguiente es un ejemplo de aplicación auxiliar de etiquetas delimitadoras que incluya el protocolo:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

Este será el código HTML generado:

```<a href="https://localhost/Home/About">About</a>```

El dominio del ejemplo es el host local, pero la aplicación auxiliar de etiquetas delimitadoras utiliza el dominio público del sitio web al generar la dirección URL.

## <a name="additional-resources"></a>Recursos adicionales

* [Áreas](xref:mvc/controllers/areas)
