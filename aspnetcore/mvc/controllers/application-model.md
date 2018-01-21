---
title: "Trabajar con el modelo de aplicación"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: c69dd1cfae713036ce0ee95f70acc162b1e82cb0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="working-with-the-application-model"></a>Trabajar con el modelo de aplicación

Por [Steve Smith](https://ardalis.com/)

Núcleo ASP.NET MVC define un *modelo de aplicación* que representan los componentes de una aplicación MVC. Puede leer y manipular el modelo para modificar cómo se comportan los elementos MVC. De forma predeterminada, MVC sigue ciertas convenciones para determinar qué clases se consideran controladores, los métodos de esas clases son las acciones y el comportan de enrutamiento y parámetros. Puede personalizar este comportamiento para satisfacer las necesidades de su aplicación mediante la creación de sus propias convenciones y aplicarla globalmente o como atributos.

## <a name="models-and-providers"></a>Modelos y los proveedores

El modelo de aplicación de MVC de ASP.NET Core incluyen interfaces abstractas y las clases de implementación concreta que describen una aplicación MVC. Este modelo es el resultado de la detección de controladores, acciones, parámetros de acción, rutas y filtros de acuerdo con las convenciones de forma predeterminada de la aplicación MVC. Cuando se trabaja con el modelo de aplicación, puede modificar la aplicación para seguir las convenciones diferentes del comportamiento de MVC predeterminada. Los parámetros, nombres, rutas y filtros usa como datos de configuración para las acciones y controladores.

El modelo de aplicación de MVC de ASP.NET Core tiene la estructura siguiente:

* ApplicationModel
    * Controladores (ControllerModel)
        * Acciones (ActionModel)
            * Parámetros (ParameterModel)

Cada nivel del modelo tiene acceso a un común `Properties` colección niveles inferiores pueden tener acceso y sobrescribir los valores de propiedad establecidos por los niveles superiores de la jerarquía. Las propiedades se conservan en el `ActionDescriptor.Properties` cuando se crean las acciones. A continuación, cuando se está controlando una solicitud, las propiedades de una convención de agregado o modificado puede obtenerse a través `ActionContext.ActionDescriptor.Properties`. Usar propiedades es una excelente manera de configurar los filtros, enlazadores de modelos, etc. según una por cada acción.

> [!NOTE]
> El `ActionDescriptor.Properties` colección no es subprocesos (para las escrituras) cuando haya finalizado el inicio de la aplicación. Las convenciones son la mejor manera de agregar datos de forma segura a esta colección.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

Núcleo de ASP.NET MVC carga el modelo de aplicación con un modelo de proveedor, definido por el [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interfaz. En esta sección se trata algunos de los detalles de la implementación interna de cómo esta funciones del proveedor. Se trata de un tema avanzado: mayoría de las aplicaciones que aprovechan el modelo de aplicación debería hacerlo cuando se trabaja con convenciones.

Las implementaciones de la `IApplicationModelProvider` interfaz "encapsular" entre sí, con cada llamada de implementación `OnProvidersExecuting` en orden ascendente según su `Order` propiedad. El `OnProvidersExecuted` en orden inverso después se llama al método. El marco de trabajo define varios proveedores:

Primer (`Order=-1000`):

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

A continuación, (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> El orden en que dos proveedores con el mismo valor para `Order` se denominan no está definido y, por tanto, no debe confiar en.

> [!NOTE]
> `IApplicationModelProvider`es un concepto avanzado para que los autores de framework extender. En general, las aplicaciones deben usar las convenciones y marcos de trabajo deben utilizar proveedores. La diferencia clave es que proveedores se ejecutan siempre antes de convenciones.

El `DefaultApplicationModelProvider` establece muchos de los comportamientos predeterminados utilizados por núcleo de ASP.NET MVC. Sus responsabilidades incluyen:

* Agregar filtros globales en el contexto
* Adición de controladores en el contexto
* Agregar métodos de controlador pública como acciones
* Agregar parámetros de método de acción en el contexto
* Aplicar la ruta y otros atributos

Algunos comportamientos integrados se implementan mediante el `DefaultApplicationModelProvider`. Este proveedor es responsable de construir el [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), que hace referencia a su vez [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), y [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instancias. La `DefaultApplicationModelProvider` clase es un detalle de implementación de marco interno que puede y cambiarán en el futuro. 

El `AuthorizationApplicationModelProvider` es responsable de aplicar el comportamiento asociado a la `AuthorizeFilter` y `AllowAnonymousFilter` atributos. [Más información acerca de estos atributos](xref:security/authorization/simple).

El `CorsApplicationModelProvider` implementa el comportamiento asociado con el `IEnableCorsAttribute` y `IDisableCorsAttribute`y el `DisableCorsAuthorizationFilter`. [Obtener más información acerca de CORS](xref:security/cors).

## <a name="conventions"></a>Convenciones

El modelo de aplicación define abstracciones de convención que proporcionan una forma más sencilla para personalizar el comportamiento de los modelos que reemplazar el modelo completo o el proveedor. Estas abstracciones son la forma recomendada para modificar el comportamiento de la aplicación. Convenciones de proporcionan una manera de escribir código que se aplicará dinámicamente las personalizaciones. Mientras [filtros](xref:mvc/controllers/filters) proporcionan un medio de modificar el comportamiento del marco, las personalizaciones le permiten controlar cómo se conecta la aplicación completa juntos.

Las convenciones siguientes están disponibles:

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

Se aplican las convenciones agregándolas a opciones de MVC o implementando `Attribute`s y aplicarla a los controladores, acciones o parámetros de acción (similar a [ `Filters` ](xref:mvc/controllers/filters)). A diferencia de los filtros, convenciones solo se ejecutan cuando se inicia la aplicación, no como parte de cada solicitud.

### <a name="sample-modifying-the-applicationmodel"></a>Ejemplo: Modificar el ApplicationModel

La siguiente convención se utiliza para agregar una propiedad en el modelo de aplicación. 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Convenciones del modelo de aplicación se aplican como opciones cuando se agrega MVC en `ConfigureServices` en `Startup`.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Las propiedades son accesibles desde el `ActionDescriptor` colección de propiedades de las acciones de controlador:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Ejemplo: Modificar la descripción de ControllerModel

Como en el ejemplo anterior, también se puede modificar el modelo de controlador para incluir propiedades personalizadas. Estos invalidará las propiedades existentes con el mismo nombre especificado en el modelo de aplicación. El atributo de convención siguiente agrega una descripción en el nivel de controlador:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Esta convención se aplica como un atributo en un controlador.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

Se tiene acceso a la propiedad "Descripción" de la misma manera como en ejemplos anteriores.

### <a name="sample-modifying-the-actionmodel-description"></a>Ejemplo: Modificar la descripción de ActionModel

Una convención de atributo independiente puede aplicarse a las acciones individuales, invalidar el comportamiento que ya se aplica en el nivel de aplicación o el controlador.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Cuando se aplica esto a una acción en el controlador del ejemplo anterior muestra cómo invalida la convención de nivel de controlador:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Ejemplo: Modificar el ParameterModel

La siguiente convención se puede aplicar a los parámetros de acción para modificar sus `BindingInfo`. La convención siguiente requiere que el parámetro sea un parámetro de ruta; se omiten otras posibles orígenes de enlace (por ejemplo, los valores de cadena de consulta).

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

El atributo se puede aplicar a cualquier parámetro de acción:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Ejemplo: Modificar el nombre de ActionModel

La convención siguiente modifica la `ActionModel` para actualizar la *nombre* de la acción a la que se aplica. El nuevo nombre se proporciona como un parámetro del atributo. Este nuevo nombre se usa al enrutar, por lo que afectará a la ruta utilizada para llegar a este método de acción.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Este atributo se aplica a un método de acción en el `HomeController`:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

Aunque es el nombre del método `SomeName`, el atributo invalida la convención MVC de usar el nombre del método y reemplaza el nombre de acción con `MyCoolAction`. Por lo tanto, la ruta utilizada para llegar a esta acción es `/Home/MyCoolAction`.

> [!NOTE]
> En este ejemplo es básicamente lo mismo que usar integrado [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) atributo.

### <a name="sample-custom-routing-convention"></a>Ejemplo: Convención de enrutamiento personalizado

Puede usar un `IApplicationModelConvention` para personalizar cómo funciona el enrutamiento. Por ejemplo, la siguiente convención incorporará los espacios de nombres de controladores en sus rutas, reemplazando `.` en el espacio de nombres con `/` en la ruta:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

La convención se agrega como una opción de inicio.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> Puede agregar una convención para su [middleware](xref:fundamentals/middleware) accediendo `MvcOptions` con`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

En este ejemplo se aplica esta convención a las rutas que no se están usando la ruta de atributo donde el controlador tiene "Namespace" en su nombre. El siguiente controlador muestra esta convención:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Uso del modelo de aplicación en WebApiCompatShim

Núcleo de ASP.NET MVC utiliza un conjunto diferente de convenciones de ASP.NET Web API 2. Mediante convenciones personalizadas, puede modificar el comportamiento de una aplicación de MVC de ASP.NET Core para que sea coherente con el de una aplicación de API de Web. Se suministra Microsoft la [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) específicamente para este propósito.

> [!NOTE]
> Obtenga más información sobre [migrar de ASP.NET Web API](xref:migration/webapi).

Para usar la corrección de compatibilidad de API de Web, debe agregar el paquete al proyecto y, a continuación, agregar las convenciones para MVC mediante una llamada a `AddWebApiConventions` en `Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

Las convenciones proporcionadas por la corrección de compatibilidad solo se aplican a las partes de la aplicación que han tenido ciertos atributos que se aplican a ellos. Los cuatro atributos siguientes se utilizan para controlar qué controladores deben tener sus convenciones de modificar las convenciones de la corrección de compatibilidad:

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Convenciones de acción

El `UseWebApiActionConventionsAttribute` se utiliza para asignar el método HTTP a las acciones según su nombre (por ejemplo, `Get` asignaría a `HttpGet`). Solo se aplica a las acciones que no utilizan la ruta de atributo.

### <a name="overloading"></a>Sobrecarga

El `UseWebApiOverloadingAttribute` se usa para aplicar la `WebApiOverloadingApplicationModelConvention` convención. Esta convención se agrega un `OverloadActionConstraint` para el proceso de selección de acción, lo que limita las acciones de candidato a aquellos para los que la solicitud cumple todos los parámetros no es opcional.

### <a name="parameter-conventions"></a>Convenciones de parámetro

El `UseWebApiParameterConventionsAttribute` se usa para aplicar la `WebApiParameterConventionsApplicationModelConvention` convención de acción. Esta convención especifica que tipos simples que se utilizan como parámetros de acción se enlazan del URI de forma predeterminada, mientras que los tipos complejos están enlazados desde el cuerpo de solicitud.

### <a name="routes"></a>Rutas

El `UseWebApiRoutesAttribute` controles si el `WebApiApplicationModelConvention` se aplica la convención de controlador. Cuando se habilita, esta convención se utiliza para agregar compatibilidad con [áreas](xref:mvc/controllers/areas) a la ruta.

Además de un conjunto de convenciones, el paquete de compatibilidad incluye un `System.Web.Http.ApiController` clase base que reemplaza proporcionado por la API Web. Esto permite que los controladores escritos para API Web y hereda de su `ApiController` para trabajar como si estuvieran diseñados, mientras se ejecuta en el núcleo de ASP.NET MVC. Esta clase de controlador base se decora con todos los `UseWebApi*` atributos mencionados anteriormente. La `ApiController` expone propiedades, métodos y tipos de resultado que son compatibles con las que se encuentran en Web API.

## <a name="using-apiexplorer-to-document-your-app"></a>Uso de ApiExplorer en la aplicación de un documento

El modelo de aplicación expone una [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) propiedad en cada nivel que puede usarse para recorrer la estructura de la aplicación. Esto se puede usar para [generar páginas de ayuda para las API Web con herramientas como Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger). El `ApiExplorer` propiedad expone un `IsVisible` propiedad que se pueden establecer para especificar qué partes del modelo de la aplicación deben exponerse. Puede configurar esta configuración mediante una convención de:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Con este enfoque (y convenciones adicionales si es necesario), puede habilitar o deshabilitar visibilidad de API en cualquier nivel dentro de la aplicación. 
