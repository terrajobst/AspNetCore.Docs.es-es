---
title: "Vistas de núcleo de ASP.NET MVC"
author: ardalis
description: "Obtenga información acerca de cómo vistas controlan la presentación de datos de la aplicación y la interacción del usuario en MVC de ASP.NET Core."
keywords: "Núcleo de ASP.NET, ver, MVC, razor, viewmodel, viewdata, viewbag"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: f40feb0466854080cc749a83c546ce857d850902
ms.sourcegitcommit: e4a1df2a5a85f299322548809e547a79b380bb92
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2017
---
# <a name="views-in-aspnet-core-mvc"></a>Vistas de núcleo de ASP.NET MVC

Por [Steve Smith](https://ardalis.com/) y [Luke Latham](https://github.com/guardrex)

En el **M**odelo -**V**er -**C**patrón ontroller (MVC), el *vista* administra la interacción de usuario y la presentación de datos de la aplicación. Una vista es una plantilla HTML con incrustado [marcado Razor](xref:mvc/views/razor). Marcado de Razor es código que interactúa con el marcado HTML para generar una página Web que se envía al cliente.

En el núcleo de ASP.NET MVC, las vistas son *.cshtml* archivos que usan el [lenguaje de programación de C#](/dotnet/csharp/) en el marcado de Razor. Por lo general, ver los archivos se agrupan en carpetas con el nombre de cada una de la aplicación [controladores](xref:mvc/controllers/actions). Las carpetas se almacenan en una en una *vistas* carpeta en la raíz de la aplicación:

![Carpeta de vistas en el Explorador de soluciones de Visual Studio está abierta con la carpeta de inicio abierta para mostrar los archivos About.cshtml, Contact.cshtml y Index.cshtml](overview/_static/views_solution_explorer.png)

El *inicio* controlador se representa mediante un *inicio* carpeta dentro de la *vistas* carpeta. El *inicio* carpeta contiene las vistas para la *sobre*, *póngase en contacto con*, y *índice* páginas Web (página principal). Cuando un usuario solicita uno de estos tres páginas Web, las acciones de controlador en el *inicio* controlador determinar cuál de las tres vistas se utiliza para crear y devolver una página Web para el usuario.

Use [diseños](xref:mvc/views/layout) para proporcionar las secciones de la página Web coherente y reducir la repetición del código. Diseños suelen contengan el encabezado, los elementos de menú y de navegación y el pie de página. El encabezado y pie de página suelen contengan marcado reutilizable para muchos elementos de metadatos y vínculos a recursos de script y el estilo. Diseños de ayudarle a evitar este marcado repetitivo en las vistas.

[Las vistas parciales](xref:mvc/views/partial) reducir la duplicación de código mediante la administración de elementos reutilizables de vistas. Por ejemplo, una vista parcial es útil para una biografía del autor en un sitio Web de blog que aparece en varias vistas. Una biografía del autor es contenido de la vista normal y no requiere código para ejecutar con el fin de generar el contenido de la página Web. Contenido de biografía del autor está disponible para la vista de forma independiente, el enlace de modelos para que el uso de una vista parcial para este tipo de contenido es ideal.

[Ver componentes](xref:mvc/views/view-components) son vistas similares en parcial que le permiten reducir código repetitivo, pero son adecuados para ver el contenido que requiere el código que se ejecuta en el servidor con el fin de representar la página Web. Permite ver los componentes son útiles cuando el contenido representado requiere la interacción de la base de datos, como para un sitio Web de carro de la compra. Componentes de la vista no están limitados para enlace de modelo para generar la salida de la página Web.

## <a name="benefits-of-using-views"></a>Ventajas del uso de vistas

Vistas ayudan a establecer una [ **S**eparation **o**f **C**oncerns (SoC) diseño](http://deviq.com/separation-of-concerns/) dentro de una aplicación MVC separando el marcado de la interfaz de usuario de otras partes de la aplicación. Después de SoC diseño hace que la aplicación modular, que ofrece varias ventajas:

* La aplicación es más fácil de mantener, ya que mejor se organizan. Vistas generalmente se agrupan por la característica de la aplicación. Resulta más fácil encontrar vistas relacionadas cuando se trabaja en una característica.
* Las partes de la aplicación no están estrechamente. Puede crear y actualizar las vistas de la aplicación por separado de los componentes de acceso lógica y los datos de negocio. Puede modificar las vistas de la aplicación sin necesidad de tener que actualizar otras partes de la aplicación.
* Es más fácil de probar los elementos de interfaz de usuario de la aplicación, ya que las vistas son unidades independientes.
* Debido a una mejor organización, es menos probable que deberá accidentalmente secciones de repetición de la interfaz de usuario.

## <a name="creating-a-view"></a>Crear una vista

Se crean vistas que son específicas para un controlador en el *vistas / [ControllerName]* carpeta. Vistas que se comparten entre los controladores se colocan en la *vistas/compartidas* carpeta. Para crear una vista, agregue un nuevo archivo y asígnele el mismo nombre que su acción de controlador asociado con la *.cshtml* la extensión de archivo. Para crear una vista para la *sobre* acción en el *inicio* controlador, cree una *About.cshtml* un archivo en el *vistas/inicio* carpeta:

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* marcado comienza con la `@` símbolos. Código de ejecución instrucciones de C# mediante la colocación de C# de [bloques de código Razor](xref:mvc/views/razor#razor-code-blocks) desactivar por llaves (`{ ... }`). Por ejemplo, vea la asignación de "About" a `ViewData["Title"]` mostrado anteriormente. Puede mostrar valores en HTML haciendo referencia simplemente el valor con el `@` símbolos. Ver el contenido de la `<h2>` y `<h3>` elementos anteriores.

El contenido de la vista mostrado anteriormente es solo una parte de toda la página Web que se presenta al usuario. El resto del diseño de la página y otros aspectos comunes de la vista se especifican en otros archivos de vista. Para obtener más información, consulte el [tema diseño](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Cómo controladores especifican vistas

Vistas normalmente se devuelven de acciones como un [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), que es un tipo de [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult). El método de acción puede crear y devolver un `ViewResult` directamente, pero que normalmente no está listo. Puesto que la mayoría de los controladores que se hereda de [controlador](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), simplemente se usa el `View` método auxiliar para devolver el `ViewResult`:

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Esta acción devuelve el *About.cshtml* se muestra en la última sección de vista se representa como la página Web siguiente:

![Acerca de la página representada en el explorador Edge](overview/_static/about-page.png)

El `View` método auxiliar tiene varias sobrecargas. También puede especificar:

* Una vista explícita para devolver:

  ```csharp
  return View("Orders");
  ```
* A [modelo](xref:mvc/models/model-binding) para pasar a la la vista:

  ```csharp
  return View(Orders);
  ```
* Una vista y un modelo:

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>Detección de vista

Cuando una acción devuelve una vista, un proceso llamado *detección de vista* tiene lugar. Este proceso determina qué archivo de vista se utiliza en función del nombre de vista. 

Cuando una acción devuelve el `View` (método) (`return View();`) y una vista no se especifica, el nombre de acción se usa como el nombre de vista. Por ejemplo, el *sobre* `ActionResult` nombre de método del controlador se usa para buscar un archivo de vista denominado *About.cshtml*. En primer lugar, el tiempo de ejecución busca en el *vistas / [ControllerName]* carpeta para la vista. Si no encuentra una vista de búsqueda de coincidencias no existe, busca la *Shared* carpeta para la vista.

No importa si implícitamente devuelve el `ViewResult` con `return View();` o pasar explícitamente el nombre de la vista a la `View` método con `return View("<ViewName>");`. En ambos casos, la detección de vista busca un archivo de vista coincidente en este orden:

   1. *Vistas /\[ControllerName]\[ViewName] .cshtml*
   1. *Vistas/compartida/\[ViewName] .cshtml*

Se puede proporcionar una ruta de acceso del archivo de vista en lugar de un nombre de vista. Si utiliza una ruta de acceso absoluta a partir de la raíz de la aplicación (si lo desea, a partir de "/" o "~ /"), el *.cshtml* extensión se debe especificar:

```csharp
return View("Views/Home/About.cshtml");
```

También puede usar una ruta de acceso relativa para especificar vistas en directorios distintos sin la *.cshtml* extensión. Dentro de la `HomeController`, puede devolver el *índice* ver de su *administrar* vistas con una ruta de acceso relativa:

```csharp
return View("../Manage/Index");
```

De forma similar, puede indicar el directorio específico del controlador actual con el ". /" prefijo:

```csharp
return View("./About");
```

[Las vistas parciales](xref:mvc/views/partial) y [ver componentes](xref:mvc/views/view-components) utilizar mecanismos de detección similares (pero no idénticos).

Puede personalizar la convención predeterminada para cómo las vistas se encuentran dentro de la aplicación mediante el comparador [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).

Detección de la vista se basa en Buscar archivos de la vista por nombre de archivo. Si el sistema de archivos subyacente distingue mayúsculas de minúsculas, nombres de las vistas son probablemente con diferenciación entre mayúsculas y minúsculas. Para la compatibilidad entre los sistemas operativos, Coincidir mayúsculas y minúsculas entre el controlador y los nombres de acción y las carpetas de la vista asociada y nombres de archivo. Si se produce un error que no se encuentra un archivo de vista mientras se trabaja con un sistema de archivos distingue mayúsculas de minúsculas, confirme que coincide con las mayúsculas y minúsculas entre el archivo de vista solicitada y el nombre de archivo real de la vista.

Siga el procedimiento recomendado de organizar la estructura de archivos para las vistas reflejar las relaciones existentes entre controladores, acciones y vistas para mayor claridad y mantenimiento.

## <a name="passing-data-to-views"></a>Pasar datos a las vistas

Puede pasar datos a vistas que utilizan varios enfoques. El enfoque más eficaz consiste en especificar un [modelo](xref:mvc/models/model-binding) tipo en la vista. Este modelo se conoce normalmente como un *viewmodel*. Pasar una instancia del tipo de modelo de vista a la vista de la acción.

Uso de un modelo de vista para pasar datos a una vista permite que la vista aprovechar las ventajas de *seguro* comprobación de tipos. *Establecimiento inflexible de tipos* (o *fuertemente tipado*) significa que cada variable y la constante tienen un tipo definido de forma explícita (por ejemplo, `string`, `int`, o `DateTime`). Se comprueba la validez de los tipos utilizados en una vista en tiempo de compilación.

Conjunto de herramientas, como [Visual Studio](https://www.visualstudio.com/vs/) o [código de Visual Studio](https://code.visualstudio.com/), también se pueden mostrar los miembros (propiedades de un modelo) mientras se agrega a una vista, lo que ayuda a escribir código más rápido con menos errores. Esta característica se denomina [IntelliSense](/visualstudio/ide/using-intellisense) en herramientas de Microsoft.

Especifique un modelo con el `@model` directiva. Utilizar el modelo con `@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Para proporcionar el modelo a la vista, el controlador lo pasa como un parámetro:

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

No hay ninguna restricción sobre los tipos de modelo que puede proporcionar a una vista. Se recomienda usar **P** **O**ld **C**LR **O**viewmodels bjeto (POCO) con poca o ninguna comportamiento (métodos) definido. Por lo general, las clases del modelo de vista se almacenan en la *modelos* carpeta o en otro *ViewModels* carpeta en la raíz de la aplicación. El *dirección* viewmodel usado en el ejemplo anterior es un viewmodel POCO almacenado en un archivo denominado *Address.cs*:

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> Nada le impide usar las mismas clases para los tipos de modelo de vista y los tipos de modelo de negocio. Sin embargo, el uso de modelos independientes permite sus vistas variar de forma independiente de la lógica de negocios y datos de partes de acceso de la aplicación. Separación de los modelos y viewmodels también ofrece ventajas de seguridad cuando los modelos utilizan [enlace de modelo](xref:mvc/models/model-binding) y [validación](xref:mvc/models/validation) para los datos enviados a la aplicación por el usuario.

### <a name="weakly-typed-data-viewdata-and-viewbag"></a>Datos débilmente tipada (ViewData y ViewBag)

Además de vistas fuertemente tipadas, vistas tienen acceso a un *débilmente tipada* (también denominada *imprecisa*) recopilación de datos. A diferencia de los tipos seguros, *tipos débiles* (o *perder tipos*) significa que no declara explícitamente el tipo de datos que esté utilizando. Puede usar la recopilación de datos débilmente tipada para pasar pequeñas cantidades de datos dentro y fuera de los controladores y vistas.

| Pasar datos entre un...                        | Ejemplo                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Controlador y una vista                             | Rellenar una lista desplegable con los datos.                                          |
| Vista y un [vista de diseño](xref:mvc/views/layout)   | Establecer el  **\<título >** contenido del elemento en la vista de diseño de un archivo de vista.  |
| [Vista parcial](xref:mvc/views/partial) y una vista | Un widget que muestra datos basados en la página Web que el usuario solicitó.      |

Puede hacer referencia a esta colección a través del `ViewData` o `ViewBag` propiedades en los controladores y vistas. El `ViewData` propiedad es un diccionario de objetos débilmente tipada. El `ViewBag` propiedad es un contenedor alrededor de `ViewData` que proporciona las propiedades dinámicas para subyacente `ViewData` colección.

`ViewData`y `ViewBag` se resuelven de forma dinámica en tiempo de ejecución. Debido a que no ofrecen la comprobación de tipos de tiempo de compilación, ambos son generalmente más propensas a errores que el uso de un modelo de vista. Por esta razón, algunos desarrolladores prefieren mínimamente o nunca `ViewData` y `ViewBag`.

**ViewData**

`ViewData`es un [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) tiene acceso a través del objeto `string` claves. Datos de cadena se pueden almacenar y utilizar directamente sin necesidad de una conversión, pero primero debe convertir otros `ViewData` valores a tipos específicos de objeto cuando se extrae de ellos. Puede usar `ViewData` para pasar datos de controladores a vistas y en las vistas, incluidos los [vistas parciales](xref:mvc/views/partial) y [diseños](xref:mvc/views/layout).

El siguiente es un ejemplo que establece los valores de un saludo y una dirección con `ViewData` en acción:

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

Trabajar con los datos en una vista:

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

**Elemento ViewBag**

`ViewBag`es un [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) objeto que proporciona acceso dinámico a los objetos almacenados en `ViewData`. `ViewBag`puede ser más cómodo trabajar con, ya que no requiere conversión. En el ejemplo siguiente se muestra cómo usar `ViewBag` con el mismo resultado que con `ViewData` anteriormente:

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**Usando ViewData y ViewBag simultáneamente**

Puesto que `ViewData` y `ViewBag` hacen referencia al mismo subyacente `ViewData` colección, puede usar ambos `ViewData` y `ViewBag` y mezclar y combinar entre ellos al leer y escribir valores.

Establecer el título con `ViewBag` y la descripción mediante `ViewData` en la parte superior de un *About.cshtml* vista:

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Leer las propiedades pero invertir el uso de `ViewData` y `ViewBag`. En el *_Layout.cshtml* de archivos, obtenga el título usando `ViewData` y obtener la descripción utilizando `ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Recuerde que las cadenas no requieren una conversión para `ViewData`. Puede usar `@ViewData["Title"]` sin conversión alguna.

Utilizar `ViewData` y `ViewBag` en el mismo funciona de tiempo, como hace mezclar y hacer coincidir la lectura y escritura de las propiedades. Se representa el marcado siguiente:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**Resumen de las diferencias entre ViewData y ViewBag**

* `ViewData`
  * Se deriva de [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), por lo que tiene propiedades de diccionario que pueden ser útiles, por ejemplo, `ContainsKey`, `Add`, `Remove`, y `Clear`.
  * Las claves del diccionario son cadenas, por lo que se permiten espacios en blanco. Ejemplo: `ViewData["Some Key With Whitespace"]`
  * Cualquier tipo excepto un `string` deben convertirse en la vista que usa `ViewData`.
* `ViewBag`
  * Se deriva de [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), por lo que permite la creación de propiedades dinámicas mediante la notación de puntos (`@ViewBag.SomeKey = <value or object>`), y no se requiere ninguna conversión. La sintaxis de `ViewBag` resulta más rápida agregar controladores y vistas.
  * Más sencillo comprobar si hay valores null. Ejemplo: `@ViewBag.Person?.Name`

**Cuándo utilizar ViewData o ViewBag**

Ambos `ViewData` y `ViewBag` son igualmente válidos enfoques para pasar pequeñas cantidades de datos entre controladores y vistas. La elección de los cuales uno para usar (o ambos) consiste en tomar una preferencia personal o la preferencia de su organización. Por lo general, los desarrolladores son coherentes en el uso de uno u otro. O bien utilizan `ViewData` everywhere o use `ViewBag` en todas partes, pero lo invitamos a mezclar y compararlos. Puesto que ambas son resuelto dinámicamente en tiempo de ejecución y, por tanto, son propensos a lo que produce errores en tiempo de ejecución, utilizarlos con cuidado. Algunos desarrolladores eviten completamente.

### <a name="dynamic-views"></a>Vistas dinámicas

El tipo de vistas que no declaran un modelo con `@model` , pero que tiene una instancia de modelo que se les pasa (por ejemplo, `return View(Address);`) puede hacer referencia a propiedades la instancia dinámicamente:

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Esta característica proporciona la flexibilidad, pero no ofrece protección de compilación ni en IntelliSense. Si la propiedad no existe, se produce un error en la generación de la página Web en tiempo de ejecución.

## <a name="more-view-features"></a>Más características de vista

[Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) resulta fácil agregar el comportamiento de servidor a las etiquetas HTML existentes. El uso de aplicaciones auxiliares de etiquetas evita la necesidad de escribir código personalizado o aplicaciones auxiliares en las vistas. Aplicaciones auxiliares de etiquetas se aplican como atributos a elementos HTML y hace caso omiso de editores que no pueden procesarlos. Esto le permite editar y representar el marcado de la vista en una variedad de herramientas.

Generar marcado HTML personalizado se puede lograr con muchas aplicaciones auxiliares de HTML integrados. Lógica de la interfaz de usuario más compleja puede administrarse mediante [ver componentes](xref:mvc/views/view-components). Ver componentes proporcionan la misma SoC que controladores y vistas ofrecen. Puede eliminar la necesidad de acciones y vistas que se encargan de datos que usa elementos comunes de la interfaz de usuario.

Al igual que muchos otros aspectos de ASP.NET Core, vistas admiten [inyección de dependencia](xref:fundamentals/dependency-injection), lo que permite a los servicios estén [insertado en vistas](xref:mvc/views/dependency-injection).
