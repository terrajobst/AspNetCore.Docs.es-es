---
title: Aplicación auxiliar de etiquetas de caché en ASP.NET Core MVC
author: pkellner
description: Muestra cómo trabajar con la aplicación auxiliar de etiqueta de caché
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 6f19a989c9bdfddea7609c5571cdd49de29e036b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898757"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Aplicación auxiliar de etiquetas de caché en ASP.NET Core MVC

Por [Peter Kellner](http://peterkellner.net) 

La aplicación auxiliar de etiqueta de caché proporciona la capacidad para mejorar drásticamente el rendimiento de la aplicación de ASP.NET Core al permitir almacenar en memoria caché su contenido en el proveedor de caché interno de ASP.NET Core.

El motor de visualización Razor establece el valor predeterminado `expires-after` en veinte minutos.

El siguiente marcado de Razor almacena en caché la fecha y hora:

```cshtml
<cache>@DateTime.Now</cache>
```

La primera solicitud a la página que contiene `CacheTagHelper` mostrará la fecha y hora actuales. Las solicitudes adicionales mostrarán el valor almacenado en caché hasta que la memoria caché expira (el valor predeterminado es 20 minutos) o se expulsa por la presión de memoria.

Puede establecer la duración de la caché con los siguientes atributos:

## <a name="cache-tag-helper-attributes"></a>Atributos de la aplicación auxiliar de etiqueta de caché

- - -

### <a name="enabled"></a>enabled    


| Tipo de atributo    | Valores válidos      |
|----------------   |----------------   |
| booleano           | "true" (valor predeterminado)  |
|                   | "false"   |


Determina si el contenido incluido en la aplicación auxiliar de etiqueta de caché se almacena en caché. El valor predeterminado es `true`.  Si se establece en `false`, la aplicación auxiliar de etiqueta de caché no tiene ningún efecto de almacenamiento en caché en la salida representada.

Ejemplo:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| Tipo de atributo |           Valor de ejemplo            |
|----------------|------------------------------------|
| DateTimeOffset | "@new DateTime(2025,1,29,17,02,0)" |

Establece una fecha de expiración absoluta. En el ejemplo siguiente, se almacenará en memoria caché el contenido de la aplicación auxiliar de etiqueta de caché hasta las 17:02 del 29 de enero de 2025.

Ejemplo:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| Tipo de atributo |        Valor de ejemplo         |
|----------------|------------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(120)" |

Establece el período de tiempo desde la primera solicitud para almacenar en caché el contenido. 

Ejemplo:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| Tipo de atributo |        Valor de ejemplo        |
|----------------|-----------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(60)" |

Establece el tiempo en que se debe expulsar una entrada de caché si no se ha accedido a ella.

Ejemplo:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| String            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

Acepta un valor de encabezado único o una lista separada por comas con los valores de encabezado que desencadenan una actualización de la caché cuando cambian. En el ejemplo siguiente se supervisa el valor del encabezado `User-Agent`. En el ejemplo se almacenará en memoria caché el contenido de cada `User-Agent` que se muestre al servidor web.

Ejemplo:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

Acepta un valor de encabezado único o una lista separada por comas con los valores de encabezado que desencadenan una actualización de la caché cuando cambia el valor de encabezado. En el ejemplo siguiente se examinan los valores de `Make` y `Model`.

Ejemplo:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

Acepta un valor de encabezado único o una lista separada por comas con los valores de encabezado que desencadenan una actualización de la caché cuando se produce un cambio en los valores de parámetro de datos de ruta. Ejemplo:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| String            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Acepta un valor de encabezado único o una lista separada por comas con los valores de encabezado que desencadenan una actualización de la caché cuando cambian los valores de encabezado. En el ejemplo siguiente se examina la cookie asociada con ASP.NET Identity. Cuando un usuario se autentica, la cookie de solicitud que se establece desencadena una actualización de la caché.

Ejemplo:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| Booleano             | "true"                  |
|                     | "false" (valor predeterminado) |

Especifica si debe restablecerse la memoria caché cuando el usuario que ha iniciado la sesión (o la entidad de seguridad del contexto) cambia. El usuario actual también se conoce como entidad de seguridad del contexto de solicitud y puede verse en una vista Razor mediante una referencia a `@User.Identity.Name`.

En el ejemplo siguiente se examina el usuario conectado actualmente.  

Ejemplo:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Con este atributo, se mantiene el contenido en caché a través de un ciclo de inicio y cierre de sesión.  Al utilizar `vary-by-user="true"`, una acción de inicio y cierre de sesión invalida la caché para el usuario autenticado.  Se invalida la memoria caché porque se genera un nuevo valor único de cookie al iniciar sesión. Se mantiene la memoria caché para el estado anónimo cuando no hay ninguna cookie o la cookie ha expirado. Esto significa que si ningún usuario ha iniciado sesión, se mantendrá la memoria caché.

- - -

### <a name="vary-by"></a>vary-by

| Tipo de atributo | Valores de ejemplo |
|----------------|----------------|
|     String     |    "@Model"    |

Permite la personalización de los datos que se almacenan en caché. Cuando el objeto al que hace referencia el valor de cadena del atributo cambia, el contenido de la aplicación auxiliar de etiqueta de caché se actualiza. A menudo se asignan a este atributo una concatenación de cadenas de valores del modelo.  De hecho, eso significa que una actualización de cualquiera de los valores concatenados invalida la memoria caché.

En el ejemplo siguiente se supone que el método de controlador que representa la vista suma el valor del entero de los dos parámetros de ruta, `myParam1` y `myParam2`, y devuelve el resultado como la propiedad de modelo simple. Cuando se cambia esta suma, el contenido de la aplicación auxiliar de etiqueta de caché se representa y almacena en caché de nuevo.  

Ejemplo:

Acción:

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

Proporciona instrucciones de expulsión de caché para el proveedor de caché integrado. El servidor web expulsará primero las entradas de caché `Low` cuando esté bajo presión de memoria.

Ejemplo:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

El atributo `priority` no garantiza un nivel específico de retención de la memoria caché. `CacheItemPriority` es solo una sugerencia. Establecer este atributo en `NeverRemove` no garantiza que siempre se conservará la memoria caché. Para más información, consulte [Recursos adicionales](#additional-resources).

La aplicación auxiliar de etiqueta de caché es dependiente del [servicio de caché de memoria](xref:performance/caching/memory). La aplicación auxiliar de etiqueta de caché agrega el servicio si no se ha agregado.

## <a name="additional-resources"></a>Recursos adicionales

* [Almacenamiento en caché en memoria](xref:performance/caching/memory)
* [Introducción a Identity](xref:security/authentication/identity)
