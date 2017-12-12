---
title: Anclar etiqueta auxiliar | Documentos de Microsoft
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiquetas de delimitador"
keywords: "ASP.NET Core, aplicación auxiliar de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: e3754c4313f01bc746ccb8efe11611ae213e3955
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="anchor-tag-helper"></a>Aplicación auxiliar de etiquetas de delimitador

Por [Peter Kellner](http://peterkellner.net) 

La aplicación auxiliar de etiquetas de delimitador mejora el delimitador HTML (`<a ... ></a>`) etiqueta mediante la adición de nuevos atributos. El vínculo generado (en la `href` etiqueta) se crea con los nuevos atributos. Esa dirección URL puede incluir un protocolo como https opcional.

El controlador de altavoz siguiente se utiliza en los ejemplos de este documento.

<br/>
**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Atributos de aplicación auxiliar de etiqueta de anclaje

- - -

### <a name="asp-controller"></a>controlador de ASP

`asp-controller`se utiliza para asociar el controlador que se usarán para generar la dirección URL. Los controladores especificados deben existir en el proyecto actual. El código siguiente enumera todos los altavoces: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

El marcado generado será:

```html
<a href="/Speaker">All Speakers</a>
```

Si el `asp-controller` se especifica y `asp-action` no lo es, el valor predeterminado `asp-action` será el método de controlador predeterminado de la vista está ejecutando actualmente. Que es, en el ejemplo anterior, si `asp-action` se omite, y se genera esta aplicación auxiliar de etiquetas de delimitador de *HomeController*del `Index` vista (**/Home**), el marcado generado será:


```html
<a href="/Home">All Speakers</a>
```

- - -
  
### <a name="asp-action"></a>acción de ASP

`asp-action`es el nombre del método de acción en el controlador que se van a incluir en los botones generados `href`. Por ejemplo, el código siguiente establece el generado `href` para que apunte a la página de detalles de altavoces:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

El marcado generado será:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Si no hay ningún `asp-controller` atributo se especifica, se usará el controlador de forma predeterminada al llamar a la vista ejecutar la vista actual.  
 
Si el atributo `asp-action` es `Index`, a continuación, no se anexa ninguna acción a la dirección URL, lo que el valor predeterminado `Index` llamada al método. La acción especificada (o su valor predeterminado), debe existir en el controlador al que hace referencia en `asp-controller`.

- - -
  
<a name="route"></a>
### <a name="asp-route-value"></a>ASP - route-{value}

`asp-route-`es un prefijo de ruta comodín. Cualquier valor que se coloca después de que el guión al final se interpretará como un parámetro de ruta posibles. Si no se encuentra una ruta predeterminada, este prefijo de ruta se anexará a la etiqueta href generada como un parámetro de solicitud y un valor. En caso contrario, se sustituirá en la plantilla de ruta.

Suponiendo que haya un método de controlador define como sigue:

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

Y haga que la plantilla de ruta predeterminados definida en su *Startup.cs* como se indica a continuación:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

El **cshtml** archivo que contiene la aplicación auxiliar etiqueta de anclaje necesarios para utilizar el **altavoces** es el parámetro de modelo que se pasan desde el controlador a la vista siguiente:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

El código HTML generado, a continuación, será la siguiente porque **identificador** se encontró en la ruta predeterminada.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Si el prefijo de ruta no forma parte de la plantilla de enrutamiento, que es el caso de los siguientes **cshtml** archivo:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

El código HTML generado, a continuación, será la siguiente porque **speakerid** no se encontró en la ruta coincida:


```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Si el valor `asp-controller` o `asp-action` no se especifica, se sigue el mismo proceso predeterminado tal y como se encuentra en la `asp-route` atributo.

- - -

### <a name="asp-route"></a>ruta de ASP

`asp-route`Proporciona una manera de crear una dirección URL que se vincula directamente a una ruta con nombre. Uso de atributos de enrutamientos, una ruta puede tener el nombre tal y como se muestra en el `SpeakerController` y se utiliza en su `Evaluations` método.

`Name = "speakerevals"`indica la etiqueta de anclaje de aplicación auxiliar para generar una ruta directa a ese método de controlador utilizando la dirección URL `/Speaker/Evaluations`. Si `asp-controller` o `asp-action` se especifica además `asp-route`, la ruta generadas no puede ser los esperados. `asp-route`no debe usarse con cualquiera de los atributos `asp-controller` o `asp-action` para evitar un conflicto de ruta.

- - -

### <a name="asp-all-route-data"></a>ASP-all-datos de ruta

`asp-all-route-data`permite crear un diccionario de pares clave / valor donde la clave es el nombre del parámetro y el valor es el valor asociado a esa clave.

Como muestra el ejemplo siguiente, se crea un diccionario en línea y los datos se pasan a la vista de razor. Como alternativa, también podrían pasar los datos con el modelo.

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

Cuando se hace clic el vínculo, el método del controlador `EvaluationsCurrent` se llama. Se denomina porque ese controlador tiene dos parámetros de cadena que coincide con lo que se ha creado a partir del `asp-all-route-data` diccionario.

Si todas las claves de la búsqueda de coincidencias de diccionario de parámetros de ruta, se sustituirán los valores en la ruta según corresponda y los demás valores no coincidentes se generará como parámetros de la solicitud.

- - -

### <a name="asp-fragment"></a>fragmento de ASP

`asp-fragment`define un fragmento de dirección URL que se anexará a la dirección URL. La aplicación auxiliar de etiquetas de delimitador agregará el carácter de almohadilla (#). Si crea una etiqueta:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

La dirección URL generada será: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Etiquetas de hash son útiles al crear aplicaciones del lado cliente. Se puede usar para marcar y buscar en JavaScript, por ejemplo fácilmente.

- - -

### <a name="asp-area"></a>área de ASP

`asp-area`establece el nombre del área que ASP.NET Core se utiliza para establecer la ruta adecuada. A continuación se muestran ejemplos de cómo el atributo de área hace una reasignación de rutas. Establecer `asp-area` a Blogs prefijos el directorio `Areas/Blogs` a las rutas de los controladores asociados y vistas de esta etiqueta delimitadora.

* Nombre de proyecto

  * *wwwroot*

  * *Áreas*

    * *Blogs*

      * *Controladores*

        * *HomeController.cs*

      * *Vistas*

        * *Página principal*

          * *Index.cshtml*
          
          * *AboutBlog.cshtml*
          
  * *Controladores*
  

        
Especificar una etiqueta de área que es válida, como ```area="Blogs"``` al hacer referencia a la ```AboutBlog.cshtml``` archivo tendrá un aspecto similar al siguiente utilizando la aplicación auxiliar de etiquetas de delimitador.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

El código HTML generado incluirá el segmento de áreas y será como se indica a continuación:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Para que las áreas MVC trabajar en una aplicación web, la plantilla de ruta debe incluir una referencia al área si existe. Esa plantilla, que es el segundo parámetro de la `routes.MapRoute` llamada al método, se mostrarán como:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`

- - -

### <a name="asp-protocol"></a>Protocolo de ASP

El `asp-protocol` es para especificar un protocolo (como `https`) en la dirección URL. Un ejemplo de aplicación auxiliar de etiquetas de delimitador que incluya el protocolo será como se indica a continuación:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

y generará HTML de la siguiente manera:

```<a href="https://localhost/Home/About">About</a>```

El dominio en el ejemplo es el host local, pero la aplicación auxiliar de etiquetas de delimitador de dominio del sitio Web público se utiliza al generar la dirección URL.

- - -

## <a name="additional-resources"></a>Recursos adicionales

* [Áreas](xref:mvc/controllers/areas)
