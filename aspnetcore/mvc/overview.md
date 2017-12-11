---
title: "Información general del núcleo de ASP.NET MVC"
author: ardalis
description: "Obtenga información acerca de cómo principales de ASP.NET MVC es un marco de trabajo para la creación de aplicaciones web y API que usan el modelo Model-View-Controller patrón de diseño."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 2492b6aa4602dbbf3b9cd3dca00d40690c640cab
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="overview-of-aspnet-core-mvc"></a>Información general del núcleo de ASP.NET MVC

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC es un completo marco de trabajo para compilar aplicaciones web y APIs mediante el patrón de diseño Modelo-Vista-Controlador.

## <a name="what-is-the-mvc-pattern"></a>¿Qué es el modelo de MVC?

El modelo de arquitectura Model-View-Controller (MVC) separa una aplicación en tres grupos principales de componentes: modelos, vistas y controladores. Este patrón ayuda a lograr [separación de intereses](http://deviq.com/separation-of-concerns/). Con este patrón, se enrutan las solicitudes de usuario a un controlador que es responsable de trabajar con el modelo para realizar las acciones del usuario y/o recuperar los resultados de consultas. El controlador elige la vista para mostrar al usuario y le proporciona los datos del modelo que requiere.

El siguiente diagrama muestra los tres componentes principales y las que hacen referencia a los demás:

![Modelo de MVC](overview/_static/mvc.png)

Este delineación de responsabilidades le ayuda a escalar la aplicación en cuanto a complejidad porque es más fácil de codificar, depurar y probar algo (modelo, vista o controlador) que tiene un único trabajo (y sigue el [principio de responsabilidad única ](http://deviq.com/single-responsibility-principle/)). Es más difícil de actualización, pruebas y código de depuración que tiene dependencias que se reparten entre dos o varias de estas tres áreas. Por ejemplo, lógica de la interfaz de usuario tiende a cambiar con mayor frecuencia que la lógica de negocios. Si la presentación código y la lógica empresarial se combina en un único objeto, tendrá que modificar un objeto que contiene la lógica de negocios cada vez que cambie la interfaz de usuario. Asi es probable que se introduzcan errores y se requiera volver a examinar de toda la lógica de negocios después de hacer un cambio minimo en cada interfaz de usuario.

> [!NOTE]
> La vista y el controlador dependen del modelo. Sin embargo, el modelo no depende de la vista ni del controlador. Esta es una de las ventajas principales de la separación. Esta separación permite compilar y probar el modelo con independencia de la presentación visual.

### <a name="model-responsibilities"></a>Responsabilidades de modelo

El modelo en una aplicación MVC representa el estado de la aplicación y cualquier lógica de negocios o las operaciones que se deben realizar. Lógica de negocios se debe encapsular en el modelo, junto con cualquier lógica de implementación para conservar el estado de la aplicación. Vistas fuertemente tipadas normalmente utilizará tipos ViewModel diseñados específicamente para que contenga los datos para mostrar en esa vista; el controlador creará y rellenar estas instancias ViewModel del modelo.

> [!NOTE]
> Hay muchas maneras de organizar el modelo en una aplicación que utiliza el modelo de arquitectura de MVC. Obtener más información sobre algunos [diferentes tipos de tipos de modelo](http://deviq.com/kinds-of-models/).

### <a name="view-responsibilities"></a>Responsabilidades de vista

Las vistas son responsables de presentar el contenido a través de la interfaz de usuario. Usan el [motor de vista Razor](#razor-view-engine) para incrustar código .NET en formato HTML. Debería haber mínimo lógica en las vistas, y debe guardar relación con toda la lógica de ellos para presentar el contenido. Si se encuentra la necesidad de realizar una gran cantidad de lógica en la vista archivos con el fin de mostrar los datos de un modelo complejo, considere el uso de un [del componente vista](views/view-components.md), ViewModel, o una plantilla de vista para simplificar la vista.

### <a name="controller-responsibilities"></a>Responsabilidades de controlador

Los controladores son los componentes que controlen la interacción del usuario, trabajan con el modelo y por último seleccionan una vista para representar. En una aplicación MVC, la vista solo muestra información; el controlador administra y responde a los proporcionados por el usuario y la interacción. En el modelo de MVC, el controlador es el punto de entrada inicial y es responsable de seleccionar qué modelo de tipos para trabajar con y qué vista se debe representar (por lo tanto, su nombre - que controla cómo la aplicación responde a una solicitud determinada).

> [!NOTE]
> Controladores no deben ser demasiado complicados por demasiados responsabilidades. Para conservar la lógica de controlador sea excesivamente compleja, use la [principio de responsabilidad única](http://deviq.com/single-responsibility-principle/) a la lógica de negocios de inserción fuera el controlador y en el modelo de dominio.

>[!TIP]
> Si encuentra que sus acciones de controlador con frecuencia realizan los mismos tipos de acciones, puede seguir el [no repita usted mismo principio](http://deviq.com/don-t-repeat-yourself/) moviendo estas acciones comunes en [filtros](#filters).

## <a name="what-is-aspnet-core-mvc"></a>¿Qué es el núcleo de ASP.NET MVC

El marco de MVC de ASP.NET Core es una ligera código abierto, marco de presentación comprobables altamente optimizado para su uso con ASP.NET Core.

Núcleo de ASP.NET MVC proporciona una manera basada en modelos para crear sitios Web dinámicos que permite una separación clara de intereses. Proporciona control total sobre el marcado, admite el desarrollo para TDD rápido y utiliza los estándares web más recientes.

## <a name="features"></a>Características

Núcleo de ASP.NET MVC incluye lo siguiente:

* [Enrutamiento](#routing)
* [Enlace de modelos](#model-binding)
* [Validación de modelos](#model-validation)
* [Inyección de dependencia](../fundamentals/dependency-injection.md)
* [Filtros](#filters)
* [Áreas](#areas)
* [API Web](#web-apis)
* [Capacidad de prueba](#testability)
* [Motor de vista Razor](#razor-view-engine)
* [Vistas fuertemente tipadas](#strongly-typed-views)
* [Aplicaciones auxiliares de etiquetas](#tag-helpers)
* [Componentes de la vista](#view-components)

### <a name="routing"></a>Enrutamiento

Núcleo ASP.NET MVC se basa en [enrutamiento de ASP.NET Core](../fundamentals/routing.md), un eficaz componente de asignación de dirección URL que le permite compilar aplicaciones que tienen direcciones URL comprensibles y que admiten búsquedas. Esto le permite definir URL modelos de nomenclatura la aplicación que funcionan bien para la optimización de motor de búsqueda (SEO) y para la generación de vínculo, sin tener en cuenta cómo se organizan los archivos en el servidor web. Puede definir las rutas mediante una sintaxis de plantilla de ruta adecuada que admite las restricciones de valores de ruta, los valores predeterminados y valores opcionales.

*Enrutamiento basado en la convención* permite definir globalmente la dirección URL de los formatos que la aplicación acepta y cómo cada uno de esos formatos se asigna a un método de acción específica en especificado controlador. Cuando se recibe una solicitud entrante, el motor de enrutamiento analiza la dirección URL coincide con a uno de los formatos de dirección URL definidos y, a continuación, llama el método de acción del controlador asociado.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*Atributo enrutamiento* le permite especificar información de enrutamiento decorando los controladores y acciones con atributos que definen las rutas de la aplicación. Esto significa que las definiciones de ruta se colocan junto al controlador y acción con la que está asociados.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>Enlace de modelos

Núcleo de ASP.NET MVC [enlace de modelo](models/model-binding.md) convierte los datos de la solicitud de cliente (valores de formulario, enrutar los datos, los parámetros de cadena de consulta, encabezados HTTP) en objetos que puede controlar el controlador. Como resultado, la lógica de controlador no tiene que realizar el trabajo de pensar en los datos de solicitud entrante; simplemente tiene los datos como parámetros a sus métodos de acción.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>validación del modelo

Núcleo ASP.NET MVC admite [validación](models/validation.md) decorando su objeto de modelo con los atributos de validación de anotación de datos. Los atributos de validación se comprueban en el cliente antes de que los valores se registran en el servidor, así como en el servidor antes de la acción de controlador se llama.

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

Una acción de controlador:

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

El marco de trabajo controlará la validación de datos de la solicitud en el cliente y en el servidor. Lógica de validación especificada en tipos de modelo se agrega a las vistas representadas como anotaciones discretas y se aplica en el explorador con [jQuery validación](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Inyección de dependencia

ASP.NET Core tiene compatibilidad integrada para [inyección de dependencia (DI)](../fundamentals/dependency-injection.md). En ASP.NET MVC de núcleo, [controladores](controllers/dependency-injection.md) puede solicitud necesita servicios a través de sus constructores, lo que les permite seguir el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/).

También puede usar su aplicación [inyección de dependencia en la vista archivos](views/dependency-injection.md), usando la `@inject` directiva:

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>Filtros

[Filtros](controllers/filters.md) ayudan a los programadores encapsular preocupaciones transversales, como control de excepciones o autorización. Filtros habilitar personalizada activo lógica previa y posteriores al procesamiento para los métodos de acción y pueden configurarse para ejecutarse en ciertos puntos dentro de la canalización de ejecución de una solicitud determinada. Los filtros pueden aplicarse a los controladores o acciones como atributos (o pueden realizarse globalmente). Varios filtros (como `Authorize`) se incluyen en el marco de trabajo.


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>Áreas

[Áreas](controllers/areas.md) proporcionan una manera de dividir una aplicación Web de MVC de ASP.NET Core grande en agrupaciones funcionales más pequeñas. Un área de forma eficaz es una estructura MVC dentro de una aplicación. En un proyecto MVC, los componentes lógicos como modelo, el controlador y la vista se guardan en carpetas diferentes y MVC usa las convenciones de nomenclatura para crear la relación entre estos componentes. Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de nivel alto de funcionalidad. Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como la desprotección, facturación y búsqueda etcetera. Cada una de estas unidades tienen sus propias vistas de componente lógico, controladores y modelos.

### <a name="web-apis"></a>API Web

Además de ser una plataforma excelente para la creación de sitios web, MVC de ASP.NET Core tiene mayor compatibilidad para la creación de las API Web. Puede crear servicios que pueden alcanzar una amplia gama de clientes, incluidos los exploradores y dispositivos móviles.

El marco incluye compatibilidad de la negociación de contenido HTTP con compatibilidad integrada para [aplicar formato a datos](models/formatting.md) como JSON o XML. Escribir [formateadores personalizados](advanced/custom-formatters.md) para agregar compatibilidad con sus propios formatos.

Utilice la generación de vínculo para habilitar la compatibilidad para hipermedia. Habilitar la compatibilidad con facilidad [recursos entre orígenes (CORS) de uso compartido](http://www.w3.org/TR/cors/) para que las API Web se pueden compartir entre varias aplicaciones Web.

### <a name="testability"></a>Capacidad de prueba

Uso del marco de trabajo de inserción de dependencias e interfaces hacer adecuadas a las pruebas unitarias y el marco de trabajo incluye características (por ejemplo, un proveedor TestHost y InMemory para Entity Framework) que hacen [las pruebas de integración](../testing/integration-testing.md) rápido y fácil así. Obtenga más información sobre [para probar la lógica de controlador](controllers/testing.md).

### <a name="razor-view-engine"></a>Motor de vista Razor

[Vistas ASP.NET MVC Core](views/overview.md) usar la [motor de vista Razor](views/razor.md) para representar vistas. Razor es un lenguaje de marcado de plantilla compact, expresivo y fluido para definir vistas que utilizan código incrustado de C#. Razor se usa para generar dinámicamente el contenido web en el servidor. Limpiamente puede mezclar código de servidor con el código y contenido del lado cliente.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

Con el motor de vista Razor, puede definir [diseños](views/layout.md), [vistas parciales](views/partial.md) y secciones reemplazables.

### <a name="strongly-typed-views"></a>Vistas fuertemente tipadas

Vistas de Razor en MVC pueden estar fuertemente tipadas en función de su modelo. Controladores pueden pasar un modelo fuertemente tipado para habilitar las vistas para que IntelliSense admite y comprobación de tipos de vistas.

Por ejemplo, la vista siguiente define un modelo de tipo `IEnumerable<Product>`:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>Aplicaciones auxiliares de etiquetas

[Aplicaciones auxiliares de etiquetas](views/tag-helpers/intro.md) habilitar el código de servidor participar en la creación y representar elementos HTML en archivos de Razor. Puede usar aplicaciones auxiliares de etiquetas para definir etiquetas personalizadas (por ejemplo, `<environment>`) o para modificar el comportamiento de las etiquetas existentes (por ejemplo, `<label>`). Aplicaciones auxiliares de etiquetas enlazar a elementos específicos que se basa en el nombre del elemento y sus atributos. Proporcionan las ventajas de la representación del lado servidor al tiempo que se mantiene una experiencia de edición HTML.

Hay muchas aplicaciones auxiliares de etiquetas integradas para las tareas comunes: como la creación de formularios, vínculos, activos de carga y los paquetes más - y aún más disponibles en repositorios públicos de GitHub y como NuGet. Aplicaciones auxiliares de etiquetas se crean en C# y se dirigen a los elementos HTML en función de nombre de elemento o atributo o etiqueta primaria. Por ejemplo, la integrada LinkTagHelper puede utilizarse para crear un vínculo a la `Login` acción de la `AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

La `EnvironmentTagHelper` puede usarse para incluir scripts diferentes en las vistas (por ejemplo, sin formato o reducida) según el entorno en tiempo de ejecución, como desarrollo, ensayo o producción:

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

Aplicaciones auxiliares de etiquetas proporcionan una experiencia de desarrollo compatible con HTML y un entorno rico de IntelliSense para crear marcado HTML y Razor. La mayoría de las aplicaciones auxiliares de etiquetas integradas elementos HTML existentes de destino y proporciona atributos de servidor para el elemento.

### <a name="view-components"></a>Componentes de la vista

[Ver componentes](views/view-components.md) permiten empaquetar la lógica de representación y volver a lo largo de la aplicación. Son similares a [vistas parciales](views/partial.md), pero con lógica asociada.
