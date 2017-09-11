---
title: "Información general de vistas"
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 7abfa7ef855eb95e1a27ba6a699dd923c9e4d7c0
ms.sourcegitcommit: 6ece943781d8a56784bb6160f14da85210d3fcea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a>Representación HTML con vistas de MVC de ASP.NET Core

Por [Steve Smith](http://ardalis.com)

Controladores de núcleo MVC de ASP.NET pueden devolver resultados con formato mediante *vistas*.

## <a name="what-are-views"></a>¿Qué son las vistas?

En el modelo Model-View-Controller (MVC), el *vista* encapsula los detalles de la presentación de la interacción del usuario con la aplicación. Las vistas son plantillas HTML con código incrustado que generan contenido para enviar al cliente. Vistas usan [sintaxis Razor](razor.md), que permite que el código para interactuar con HTML con un mínimo de código o ceremonia.

Vistas ASP.NET MVC Core son *.cshtml* archivos almacenados de forma predeterminada en un *vistas* carpeta dentro de la aplicación. Normalmente, cada controlador tendrá su propia carpeta, en el que son para las acciones de un controlador específico.

![Carpeta de vistas en el Explorador de soluciones](overview/_static/views_solution_explorer.png)

Además de las vistas específicas de la acción, [vistas parciales](partial.md), [diseños y otros archivos de vista especial](layout.md) puede utilizarse para ayudar a reducir la repetición y permitir su reutilización en las vistas de la aplicación.

## <a name="benefits-of-using-views"></a>Ventajas del uso de vistas

Las vistas proporcionan [separación de intereses](http://deviq.com/separation-of-concerns/) dentro de una aplicación MVC, la encapsulación de marcado de nivel de interfaz de usuario por separado de la lógica de negocios. Vistas de MVC de ASP.NET usan [sintaxis Razor](razor.md) para agilizar el cambio entre HTML marcado y servidor lógica sencilla. Aspectos comunes y repetitivas de interfaz de usuario de la aplicación pueden ser reutilizados entre vistas con [diseño y directivas compartidas](layout.md) o [vistas parciales](partial.md).

## <a name="creating-a-view"></a>Crear una vista

Se crean vistas que son específicas para un controlador en el *vistas / [ControllerName]* carpeta. Vistas que se comparten entre los controladores se colocan en la */vistas/Shared* carpeta. Asignar nombre al archivo de vista igual que su acción de controlador asociado y agregue el *.cshtml* la extensión de archivo. Por ejemplo, para crear una vista para la *sobre* acción en el *inicio* controlador, debe crear el *About.cshtml* un archivo en el  * /vistas/inicio*carpeta.

Un ejemplo de archivo de vista (*About.cshtml*):

[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* código se indica mediante el `@` símbolos. Instrucciones de C# se ejecutan dentro de bloques de código desactivar llaves de Razor (`{` `}`), como la asignación de "About" para el `ViewData["Title"]` elemento mostrado anteriormente. Razor puede utilizarse para mostrar valores en HTML haciendo referencia simplemente el valor con el `@` de símbolos, como se muestra en el `<h2>` y `<h3>` elementos anteriores.

Esta vista se centra en sólo la parte de la salida para el que es responsable. El resto del diseño de la página y otros aspectos comunes de la vista, se especifican en otro lugar. Obtenga más información sobre [diseño y la lógica de vista compartida](layout.md).

## <a name="how-do-controllers-specify-views"></a>¿Cómo especificar vistas de controladores?

Vistas normalmente se devuelven de acciones como un `ViewResult`. El método de acción puede crear y devolver un `ViewResult` directamente, pero normalmente si el controlador se hereda de `Controller`, simplemente deberá usar el `View` método auxiliar, como en este ejemplo se muestra:

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

El `View` método auxiliar tiene varias sobrecargas para facilitar la devolución de vistas para los desarrolladores de aplicaciones. También puede especificar una vista para devolver, así como un objeto de modelo para pasar a la vista.

Esta acción devuelve el *About.cshtml* se representa la vista mostrada anteriormente:

![Acerca de la página](overview/_static/about-page.png)

### <a name="view-discovery"></a>Detección de vista

Cuando una acción devuelve una vista, un proceso llamado *detección de vista* tiene lugar. Este proceso determina qué archivo de vista se usará. A menos que se especifica un archivo de vista específicos, el runtime busca una vista específica del controlador, a continuación, buscará en primer nombre de la vista de búsqueda de coincidencias en la *Shared* carpeta.

Cuando una acción devuelve el `View` método, como `return View();`, el nombre de acción se usa como el nombre de vista. Por ejemplo, si esto se le llama desde un método de acción denominado "Índice", sería equivalente a pasar un nombre de vista de "Índice". Un nombre de vista se puede pasar explícitamente al método (`return View("SomeView");`). En ambos casos, ver detección busca un archivo de vista de búsqueda de coincidencias en:

   1. Vistas /\<ControllerName > /\<ViewName > .cshtml

   2. Vistas/compartida/\<ViewName > .cshtml

>[!TIP]
> Recomendamos seguir la convención de devolver simplemente `View()` de acciones siempre que sea posible, como resultado más flexible y fácil a refactorizar el código.

Se puede proporcionar una ruta de acceso del archivo de vista en lugar de un nombre de vista. Si utiliza una ruta de acceso absoluta a partir de la raíz de la aplicación (si lo desea, a partir de "/" o "~ /"), el *.cshtml* extensión debe especificarse como parte de la ruta de acceso de archivo (por ejemplo, `return View("Views/Home/About.cshtml");`). Como alternativa, puede usar una ruta de acceso relativa desde el directorio específico del controlador dentro de la *vistas* directorio para especificar vistas en directorios diferentes (por ejemplo, `return View("../Manage/Index");` dentro de la `HomeController`). Del mismo modo, puede recorrer el directorio específico del controlador actual (por ejemplo, `return View("./About");`). Tenga en cuenta que no use rutas de acceso relativas del *.cshtml* extensión. Como se mencionó anteriormente, siga el procedimiento recomendado de organizar la estructura de archivos para las vistas reflejar las relaciones existentes entre controladores, acciones y vistas para mayor claridad y mantenimiento.

> [!NOTE]
> [Las vistas parciales](partial.md) y [ver componentes](view-components.md) utilizar mecanismos de detección similares (pero no idénticos).

> [!NOTE]
> Puede personalizar la convención predeterminada con respecto a dónde se encuentran dentro de la aplicación vistas mediante el comparador `IViewLocationExpander`.

>[!TIP]
> Nombres de vista pueden ser entre mayúsculas y minúsculas según el sistema de archivos subyacente. Para la compatibilidad entre los sistemas operativos, siempre Coincidir mayúsculas y minúsculas entre el controlador y los nombres de acción y las carpetas de la vista asociada y nombres de archivo.

## <a name="passing-data-to-views"></a>Pasar datos a las vistas

Puede pasar datos a vistas que utilizan varios mecanismos. El enfoque más eficaz consiste en especificar un *modelo* tipo en la vista (suele conocerse como un *viewmodel*, para distinguirlo de los tipos de modelo de dominio de negocio) y, a continuación, pase una instancia de este tipo a la vista de la acción. Se recomienda que usar un modelo o el modelo de vista para pasar datos a una vista. Esto permite que la vista aprovechar las ventajas de la comprobación de tipo seguro. Puede especificar un modelo para una vista mediante el `@model` directiva:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

Una vez que se ha especificado un modelo para obtener una vista, puede tener acceso a la instancia que se envían a la vista en un modo fuertemente tipado mediante `@Model` como se indicó anteriormente. Para proporcionar una instancia del tipo de modelo a la vista, el controlador lo pasa como un parámetro:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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

No hay ninguna restricción sobre los tipos que se pueden proporcionar para una vista como un modelo. Se recomienda pasar objetos de CLR antiguos sin formato (POCO) ver modelos con el comportamiento de poca o ninguna, por lo que la lógica de negocios se puede encapsular en otra parte de la aplicación. Un ejemplo de este enfoque es la *dirección* viewmodel usado en el ejemplo anterior:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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
> Nada le impide usar las mismas clases como sus tipos de modelo de negocio y los tipos de modelo de presentación. Sin embargo, consigue que sigan siendo independientes permite a las vistas variar de forma independiente desde el modelo de dominio o la persistencia y puede ofrecer también algunas ventajas de seguridad (para los modelos que se enviarán a los usuarios a la aplicación con [enlace de modelo](../models/model-binding.md)).

### <a name="loosely-typed-data"></a>Datos débilmente tipadas

Además de vistas fuertemente tipadas, todas las vistas tienen acceso a una colección de datos débilmente tipada. Puede hacer referencia a esta misma colección a través del `ViewData` o `ViewBag` propiedades en los controladores y vistas. El `ViewBag` propiedad es un contenedor alrededor de `ViewData` que proporciona una vista dinámica con respecto a esa colección. No es una colección independiente.

`ViewData`es un objeto de diccionario tiene acceso a través de `string` claves. Puede almacenar y recuperar los objetos en ella, y deberá convertirlos a un tipo específico cuando se extrae de ellos. Puede usar `ViewData` para pasar datos de un controlador de vistas, así como en vistas (vistas parciales y diseños). Datos de cadena se pueden almacenar y utilizar directamente, sin necesidad de una conversión de tipos.

Establecer algunos valores para `ViewData` en acción:

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

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

El `ViewBag` objetos proporciona acceso dinámico a los objetos almacenados en `ViewData`. Esto puede ser más cómodo trabajar con, ya que no requiere conversión. El mismo ejemplo que antes, con `ViewBag` en lugar de un fuertemente tipado `address` instancia en la vista:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> Dado que ambos hacen referencia al mismo subyacente `ViewData` colección, puede mezclar y combinar entre `ViewData` y `ViewBag` al leer y escribir valores, si resulta adecuado.

### <a name="dynamic-views"></a>Vistas dinámicas

Vistas que no se declaran un tipo de modelo, pero tienen una instancia de modelo que se les pasan pueden hacer referencia a esta instancia dinámicamente. Por ejemplo, si una instancia de `Address` se pasa a una vista que no declara un `@model`, todavía sería podemos hacer referencia a las propiedades de la instancia dinámicamente a medida que se muestra la vista:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

Esta característica puede ofrecer cierta flexibilidad, pero no proporciona ninguna protección de compilación ni en IntelliSense. Si la propiedad no existe, se producirá un error en la página en tiempo de ejecución.

## <a name="more-view-features"></a>Más características de vista

[Aplicaciones auxiliares de etiquetas](tag-helpers/intro.md) resulta fácil agregar el comportamiento de servidor a las etiquetas HTML existentes, evitando la necesidad de utilizar código personalizado o aplicaciones auxiliares en las vistas. Aplicaciones auxiliares de etiquetas se aplican como atributos a elementos HTML, que hace caso omiso de editores que no están familiarizados con ellas, lo que permite ver las marcas editarse y representan en una variedad de herramientas. Aplicaciones auxiliares de etiquetas tienen muchos usos y, en concreto puede realizar [trabajar con formularios](working-with-forms.md) mucho más fácil.

Generar marcado HTML personalizado se puede lograr con muchas aplicaciones auxiliares de HTML integrados y una lógica más compleja de interfaz de usuario (posiblemente con sus propios requisitos de datos) se puede encapsular en [ver componentes](view-components.md). Componentes de la vista proporcionan la misma separación de aspectos que ofrecen controladores y vistas y pueden eliminar la necesidad de acciones y vistas para tratar con los datos usados por los elementos de interfaz de usuario comunes.

Al igual que muchos otros aspectos de ASP.NET Core, vistas admiten [inyección de dependencia](../../fundamentals/dependency-injection.md), lo que permite a los servicios estén [insertado en vistas](dependency-injection.md).
