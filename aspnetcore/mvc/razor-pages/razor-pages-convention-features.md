---
title: "Razor páginas ruta y la aplicación convención características de ASP.NET Core"
author: guardrex
description: "Descubra cómo enrutar y aplicación características convención del modelo proveedor ayudar a procesamiento, detección y control página enrutamiento."
ms.author: riande
manager: wpickett
ms.date: 10/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 69475ca9abd4e732dc704ad6a8a2fffe219984f7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Razor páginas ruta y la aplicación convención características de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Aprenda a usar página ruta y la aplicación de proveedor convención características del modelo para controlar el enrutamiento de la página, la detección y el procesamiento en las aplicaciones de las páginas de Razor. Cuando necesite configurar rutas de una página personalizada para páginas individuales, configurar el enrutamiento a las páginas con la [AddPageRoute convención](#configure-a-page-route) se describe más adelante en este tema.

Use la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) para explorar las características descritas en este tema.

| Características | El ejemplo muestra cómo... |
| -------- | --------------------------- |
| [Convenciones de modelo de ruta y la aplicación](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Agregar una plantilla de ruta y un encabezado para las páginas de una aplicación. |
| [Convenciones de acción de ruta de página](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Adición de una plantilla de ruta a las páginas en una carpeta y a una sola página. |
| [Convenciones de acción del modelo de página](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (clase de filtro, expresión lambda o fábrica de filtro)</li></ul> | Agregar un encabezado a páginas en una carpeta, agregar un encabezado a una sola página y configurar un [la fábrica del filtro](xref:mvc/controllers/filters#ifilterfactory) para agregar un encabezado a las páginas de una aplicación. |
| [Proveedor de modelo de aplicación de página predeterminado](#replace-the-default-page-app-model-provider) | Reemplazar el proveedor del modelo de página predeterminado para cambiar las convenciones de nomenclatura de controlador. |

## <a name="add-route-and-app-model-conventions"></a>Agregar la ruta y la aplicación convenciones de modelo

Agregue un delegado para [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) para agregar la ruta y la aplicación convenciones de modelo que se aplican a las páginas de Razor.

**Agregar una convención de modelo de ruta a todas las páginas**

Use [convenciones](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para crear y agregar un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) a la colección de [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instancias que se aplican durante el modelo de ruta y la página construcción.

La aplicación de ejemplo agrega un `{globalTemplate?}` plantilla de ruta para todas las páginas en la aplicación:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> El `Order` propiedad para la `AttributeRouteModel` se establece en `0` (cero). Esto garantiza que esta plantilla se dan prioridad para la primera posición del valor de datos de ruta cuando se proporciona un valor de ruta. Por ejemplo, el ejemplo agrega un `{aboutTemplate?}` plantilla de ruta posteriormente en este tema. El `{aboutTemplate?}` plantilla tiene un `Order` de `1`. Cuando se solicita la página About `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 0`) y no `RouteData.Values["aboutTemplate"]` (`Order = 1`) debido a la configuración del `Order` propiedad.

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

Página de información acerca del ejemplo en solicitud `localhost:5000/About/GlobalRouteValue` e inspeccionar el resultado:

![Se solicitó la página acerca con un segmento de ruta de GlobalRouteValue. La página representada muestra que el valor de datos de ruta se captura en el método OnGet de la página.](razor-pages-convention-features/_static/about-page-global-template.png)

**Agregar una convención de modelo de aplicación a todas las páginas**

Use [convenciones](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para crear y agregar un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) a la colección de [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instancias que se aplican durante la ruta y la página construcción de modelo.

Para demostrar esta y otras convenciones posteriormente en este tema, la aplicación de ejemplo incluye una `AddHeaderAttribute` clase. El constructor de clase acepta un `name` cadena y un `values` matriz de cadenas. Estos valores se usan en su `OnResultExecuting` método para establecer un encabezado de respuesta. La clase completa se muestra en el [página convenciones de acción de modelo](#page-model-action-conventions) sección posteriormente en este tema.

La aplicación de ejemplo se usa el `AddHeaderAttribute` clase para agregar un encabezado, `GlobalHeader`, para todas las páginas en la aplicación:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

Página de información acerca del ejemplo en solicitud `localhost:5000/About` e inspeccionar los encabezados para ver el resultado:

![Encabezados de respuesta de la página acerca de muestran que se ha agregado el GlobalHeader.](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>Convenciones de acción de ruta de página

El proveedor de modelo de ruta predeterminado que se deriva de [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) convenciones que están diseñadas para proporcionar puntos de extensibilidad para configurar las rutas de la página, se invoca.

**Convención de modelo de ruta de carpeta**

Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) para crear y agregar un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoca una acción en el [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) para todas las páginas en el carpeta especificada.

Los usos de la aplicación de ejemplo `AddFolderRouteModelConvention` para agregar una `{otherPagesTemplate?}` plantilla de ruta para las páginas en el *OtherPages* carpeta:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> El `Order` propiedad para la `AttributeRouteModel` está establecido en `1`. Esto garantiza que la plantilla para `{globalTemplate?}` (configurada anteriormente en el tema) tiene prioridad para los datos de ruta primer valor posición cuando se proporciona un valor de ruta. Si se solicita la página Page1 `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 0`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) debido a la configuración del `Order` propiedad.

Solicitud Page1 subpágina del ejemplo `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` e inspeccionar el resultado:

![Se solicitó Page1 en la carpeta OtherPages con un segmento de ruta de GlobalRouteValue y OtherPagesRouteValue. La página representada muestra que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**Convención de modelo de ruta de página**

Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) para crear y agregar un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoca una acción en el [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) para la página con los valores especificados nombre.

Los usos de la aplicación de ejemplo `AddPageRouteModelConvention` para agregar una `{aboutTemplate?}` plantilla de ruta en la página acerca de:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> El `Order` propiedad para la `AttributeRouteModel` está establecido en `1`. Esto garantiza que la plantilla para `{globalTemplate?}` (configurada anteriormente en el tema) tiene prioridad para los datos de ruta primer valor posición cuando se proporciona un valor de ruta. Si se solicita la página About `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 0`) y no `RouteData.Values["aboutTemplate"]` (`Order = 1`) debido a la configuración del `Order` propiedad.

Página de información acerca del ejemplo en solicitud `localhost:5000/About/GlobalRouteValue/AboutRouteValue` e inspeccionar el resultado:

![Acerca de la página se solicitó con segmentos de ruta para GlobalRouteValue y AboutRouteValue. La página representada muestra que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Configurar una ruta de la página

Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) para configurar una ruta a una página en la ruta de acceso de la página especificada. Vínculos generados a la página de utilizar la ruta especificada. `AddPageRoute`utiliza `AddPageRouteModelConvention` para establecer la ruta.

La aplicación de ejemplo crea una ruta al `/TheContactPage` para *Contact.cshtml*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

También puede alcanzar la página de contacto en `/Contact` a través de su ruta predeterminada.

Ruta personalizada de la aplicación de ejemplo a la página de contacto permite opcional `text` segmento de ruta (`{text?}`). La página también incluye este segmento opcional en su `@page` directiva en caso de que el visitante tiene acceso a la página en su `/Contact` ruta:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

Tenga en cuenta que la dirección URL se genera para la **póngase en contacto con** vínculo en la página presentada refleja la ruta actualizada:

![Vínculo de contacto de aplicación de ejemplo en la barra de navegación](razor-pages-convention-features/_static/contact-link.png)

![Inspeccionar el vínculo Póngase en contacto con en el HTML representado indica que el href está establecido en ' / TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

Visite la página de contacto en cualquiera de su ruta normal, `/Contact`, o la ruta personalizada, `/TheContactPage`. Si se proporcionan más `text` segmento de ruta, la página muestra el segmento codificada en HTML que proporcionan:

![Ejemplo de explorador del borde de proporcionar un segmento de ruta opcional 'text' de 'Texto' en la dirección URL. La página representada muestra el valor del segmento 'text'.](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenciones de acción del modelo de página

El proveedor de modelo de página predeterminado que implementa [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) convenciones que están diseñadas para proporcionar puntos de extensibilidad para la configuración de modelos de página, se invoca. Estas convenciones son útiles cuando la creación y modificación de detección de página y las características de procesamiento.

Los ejemplos de esta sección, la aplicación de ejemplo usa un `AddHeaderAttribute` (clase), que es un [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), que aplica un encabezado de respuesta:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

Usa las convenciones, el ejemplo muestra cómo aplicar el atributo a todas las páginas en una carpeta y a una sola página.

**Convención de modelo de aplicación de carpeta**

Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) para crear y agregar un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoca una acción en [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instancias todas las páginas en la carpeta especificada.

El ejemplo muestra el uso de `AddFolderApplicationModelConvention` mediante la adición de un encabezado, `OtherPagesHeader`, a las páginas dentro de la *OtherPages* carpeta de la aplicación:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

Solicitud Page1 subpágina del ejemplo `localhost:5000/OtherPages/Page1` e inspeccionar los encabezados para ver el resultado:

![Encabezados de respuesta de la página de OtherPages/Page1 mostrar que se ha agregado el OtherPagesHeader.](razor-pages-convention-features/_static/page1-otherpages-header.png)

**Convención de modelo de aplicación de página**

Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) para crear y agregar un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoca una acción en el [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) para la página con el nombre speciifed.

El ejemplo muestra el uso de `AddPageApplicationModelConvention` mediante la adición de un encabezado, `AboutHeader`, a la página acerca de:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

Página de información acerca del ejemplo en solicitud `localhost:5000/About` e inspeccionar los encabezados para ver el resultado:

![Encabezados de respuesta de la página acerca de muestran que se ha agregado el AboutHeader.](razor-pages-convention-features/_static/about-page-about-header.png)

**Configurar un filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configura aplicar el filtro especificado. Puede implementar una clase de filtro, pero la aplicación de ejemplo muestra cómo implementar un filtro en una expresión lambda, que se implementa en segundo plano como un generador que devuelva un filtro:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

El modelo de páginas de aplicación se usa para comprobar la ruta de acceso relativa para los segmentos que conducen a la página de Page2 en el *OtherPages* carpeta. Si pasa la condición, se agrega un encabezado. Si no es así, el `EmptyFilter` se aplica.

`EmptyFilter`es un [filtro de acción](xref:mvc/controllers/filters#action-filters). Puesto que las páginas de Razor, omite los filtros de acción el `EmptyFilter` no ops según lo previsto si no contiene la ruta de acceso `OtherPages/Page2`.

Página de Page2 del ejemplo en solicitud `localhost:5000/OtherPages/Page2` e inspeccionar los encabezados para ver el resultado:

![El OtherPagesPage2Header se agrega a la respuesta para Page2.](razor-pages-convention-features/_static/page2-filter-header.png)

**Configurar una fábrica de filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configura el generador especificado para aplicar [filtros](xref:mvc/controllers/filters) a todas las páginas de Razor.

La aplicación de ejemplo muestra un ejemplo del uso de un [la fábrica del filtro](xref:mvc/controllers/filters#ifilterfactory) mediante la adición de un encabezado, `FilterFactoryHeader`, con dos valores a las páginas de la aplicación:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Página de información acerca del ejemplo en solicitud `localhost:5000/About` e inspeccionar los encabezados para ver el resultado:

![Encabezados de respuesta de la página acerca de muestran que se han agregado dos encabezados FilterFactoryHeader.](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Reemplace el proveedor de modelo de aplicación de página predeterminado

Las páginas de Razor usa el `IPageApplicationModelProvider` interfaz para crear un [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Puede heredar del proveedor de modelo predeterminado para proporcionar su propia lógica de implementación para el procesamiento y detección de controlador. La implementación predeterminada ([origen de referencia](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establece convenciones para *sin nombre* y *denominado* controlador nombres, que se describe a continuación.

**Métodos de controlador sin nombre predeterminados**

Los métodos de control para los verbos HTTP (métodos de controlador "sin nombre") siguen una convención: `On<HTTP verb>[Async]` (anexar `Async` es opcional pero recomendado para los métodos asincrónicos).

| Método de controlador sin nombre     | Operación                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inicializar el estado de la página.     |
| `OnPost`/`OnPostAsync`     | Controlar las solicitudes POST.          |
| `OnDelete`/`OnDeleteAsync` | Controlar las solicitudes de eliminación &#8224;. |
| `OnPut`/`OnPutAsync`       | Controlar las solicitudes PUT &#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Controlar las solicitudes de revisión &#8224;.  |

&#8224; Se utiliza para realizar llamadas a la API a la página.

**Con el nombre de los métodos de control predeterminada**

Métodos de controlador proporcionados por el programador ("denominado" métodos de controlador) siguen una convención similar. El nombre del controlador aparece tras el verbo HTTP o entre el verbo HTTP y `Async`: `On<HTTP verb><handler name>[Async]` (anexar `Async` es opcional pero recomendado para los métodos asincrónicos). Por ejemplo, los métodos que procesan los mensajes pueden tardar la nomenclatura se muestra en la tabla siguiente.

| En el ejemplo se denomina método de controlador             | Operación del ejemplo        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Obtener un mensaje.        |
| `OnPostMessage`/`OnPostMessageAsync`     | EXPONER un mensaje.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | ELIMINAR un mensaje &#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | Coloque un mensaje &#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Aplicar la revisión de un mensaje &#8224;.  |

&#8224; Se utiliza para realizar llamadas a la API a la página.

**Personalizar los nombres de método de controlador**

Suponga que desea cambiar la forma en que se denominan métodos de controlador con nombre y. Un esquema de nomenclatura alternativo consiste en evitar a partir de los nombres de método con "On" y usar el primer segmento de word para determinar el verbo HTTP. Puede realizar otros cambios, como la conversión de los verbos para DELETE, colocar y aplicar la revisión a POST. Un esquema de este tipo proporciona los nombres de método se muestra en la tabla siguiente.

| Método de controlador                       | Operación                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inicializar el estado de la página.     |
| `Post`/`PostAsync`                   | Controlar las solicitudes POST.          |
| `Delete`/`DeleteAsync`               | Controlar las solicitudes de eliminación &#8224;. |
| `Put`/`PutAsync`                     | Controlar las solicitudes PUT &#8224;.    |
| `Patch`/`PatchAsync`                 | Controlar las solicitudes de revisión &#8224;.  |
| `GetMessage`                         | Obtener un mensaje.              |
| `PostMessage`/`PostMessageAsync`     | EXPONER un mensaje.                |
| `DeleteMessage`/`DeleteMessageAsync` | EXPONER un mensaje para eliminar.      |
| `PutMessage`/`PutMessageAsync`       | EXPONER un mensaje para colocar.         |
| `PatchMessage`/`PatchMessageAsync`   | EXPONER un mensaje en la revisión.       |

&#8224; Se utiliza para realizar llamadas a la API a la página.

Para establecer este esquema, heredan de la `DefaultPageApplicationModelProvider` clase e invalidar el [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) método para proporcionar lógica personalizada para resolver [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) nombres de controlador. La aplicación de ejemplo muestra cómo se hace su `CustomPageApplicationModelProvider` clase:

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Aspectos destacados de la clase se incluyen:

* La clase hereda de `DefaultPageApplicationModelProvider`.
* El `TryParseHandlerMethod` procesa un controlador para determinar el verbo HTTP (`httpMethod`) y el nombre de controlador con nombre (`handlerName`) al crear la `PageHandlerModel`.
  * Un `Async` postfijo se omite, si está presente.
  * Mayúsculas y minúsculas se utilizan para analizar el verbo HTTP desde el nombre del método.
  * Cuando el nombre del método (sin `Async`) es igual que el nombre del verbo HTTP, no hay ningún controlador con nombre. El `handlerName` está establecido en `null`, y es el nombre del método `Get`, `Post`, `Delete`, `Put`, o `Patch`.
  * Cuando el nombre del método (sin `Async`) es mayor que el nombre del verbo HTTP, hay un controlador con nombre. La propiedad `handlerName` se establece como `<method name (less 'Async', if present)>`. Por ejemplo, "GetMessage" y "GetMessageAsync" producen un nombre de controlador de "GetMessage".
  * Verbos HTTP PATCH, PUT y DELETE se convierten en POST.

Registrar el `CustomPageApplicationModelProvider` en la `Startup` clase:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

El archivo de código subyacente *Index.cshtml.cs* muestra cómo se cambian las convenciones de nomenclatura de método de controlador normal de las páginas de la aplicación. Se quita la normal "En" prefijo nomenclatura usarla con páginas de Razor. El método que inicializa el estado de página ahora se denomina `Get`. Puede ver esta convención utiliza a lo largo de la aplicación si abrir cualquier archivo de código subyacente para cualquiera de las páginas.

Cada uno de los otros métodos de inicio con el verbo HTTP que describe su procesamiento. Los dos métodos que empiezan por `Delete` normalmente se trataría como verbos HTTP DELETE, pero la lógica de `TryParseHandlerMethod` se establece explícitamente el verbo POST para ambos controladores.

Tenga en cuenta que `Async` es opcional entre `DeleteAllMessages` y `DeleteMessageAsync`. Son ambos métodos asincrónicos, pero se puede usar el `Async` de postfijo o no; se recomienda que realice. `DeleteAllMessages`se usa aquí para fines de demostración, pero se recomienda que asigne nombre a este tipo de método `DeleteAllMessagesAsync`. No se ve afectada el procesamiento de la muestra de la implementación, pero su uso la `Async` postfijo llamadas el hecho de que es un método asincrónico.

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Tenga en cuenta los nombres de controlador proporcionados en *Index.cshtml* coincide con la `DeleteAllMessages` y `DeleteMessageAsync` los métodos de control:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async`en el nombre del método de controlador `DeleteMessageAsync` incluir alejar por la `TryParseHandlerMethod` para la coincidencia de controlador de la solicitud POST al método. El `asp-page-handler` nombre de `DeleteMessage` coincide con el método de controlador `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtros de MVC y el filtro de página (IPageFilter)

MVC [filtros de acción](xref:mvc/controllers/filters#action-filters) hace caso omiso de las páginas de Razor, dado que las páginas de Razor usa los métodos de control. Otros tipos de filtros MVC están disponibles para su uso: [autorización](xref:mvc/controllers/filters#authorization-filters), [excepción](xref:mvc/controllers/filters#exception-filters), [recursos](xref:mvc/controllers/filters#resource-filters), y [resultado](xref:mvc/controllers/filters#result-filters). Para obtener más información, consulte el [filtros](xref:mvc/controllers/filters) tema.

El filtro de página ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) es un filtro que se aplica a las páginas de Razor. Que rodea la ejecución de un método de controlador de la página. Permite procesar código personalizado en fases de ejecución del método de controlador de página. Este es un ejemplo de la aplicación de ejemplo:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

Este filtro busca un `globalTemplate` enrutar el valor de "TriggerValue" e intercambios en "Valordereemplazo".

El `ReplaceRouteValueFilter` atributo se puede aplicar directamente a un `PageModel` en el código subyacente:

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

Solicite la página McAfee3 desde la aplicación de ejemplo con en `localhost:5000/OtherPages/Page3/TriggerValue`. Tenga en cuenta cómo el filtro reemplaza el valor de ruta:

![La solicitud para OtherPages/página3 con un resultado del segmento de ruta TriggerValue en el filtro reemplazando el valor de ruta con un valor de reemplazo.](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>Vea también

* [Convenciones de autorización de las páginas de Razor](xref:security/authorization/razor-pages-authorization)
