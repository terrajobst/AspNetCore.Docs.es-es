---
title: "Almacenar en caché auxiliar de etiqueta en el núcleo de ASP.NET MVC"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiqueta de caché"
keywords: "ASP.NET Core, aplicación auxiliar de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: da5b7b3bf1aa01ee22edf9bd003d8f79a00a5d0b
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Almacenar en caché auxiliar de etiqueta en el núcleo de ASP.NET MVC

Por [Peter Kellner](http://peterkellner.net) 


La aplicación auxiliar de etiqueta de caché proporciona la capacidad para mejorar drásticamente el rendimiento de la aplicación de ASP.NET Core almacenando en memoria caché su contenido para el proveedor de caché de ASP.NET Core interno.

El motor de vista Razor establece el valor predeterminado `expires-after` veinte minutos.

El siguiente marcado de Razor almacena en caché la fecha y hora:

```cshtml
<Cache>@DateTime.Now<Cache>
```

La primera solicitud a la página que contiene `CacheTagHelper` mostrará la fecha y hora actuales. Las solicitudes adicionales mostrarán el valor almacenado en caché hasta que la memoria caché expira (el valor predeterminado es 20 minutos) o se expulsa a la presión de memoria.

Puede establecer la duración de la caché con los siguientes atributos:

## <a name="cache-tag-helper-attributes"></a>Atributos de la aplicación auxiliar de etiqueta de caché

- - -

### <a name="enabled"></a>enabled    


| Tipo de atributo    | Valores válidos      |
|----------------   |----------------   |
| booleano           | "true" (valor predeterminado)  |
|                   | "false"   |


Determina si el contenido incluido en la aplicación auxiliar de etiqueta de caché se almacena en caché. De manera predeterminada, es `true`.  Si establece en `false` esta aplicación auxiliar de etiqueta de caché no tiene ningún efecto de almacenamiento en caché en la salida representada.

Ejemplo:

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a>caduca en 

| Tipo de atributo    | Valor de ejemplo     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


Establece una fecha de expiración absoluta. En el ejemplo siguiente, se almacenará en memoria caché el contenido de la aplicación auxiliar de etiqueta de caché hasta 5:02 P.M. el 29 de enero de 2025.

Ejemplo:

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a>expira después

| Tipo de atributo    | Valor de ejemplo     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(120)"    |


Establece el período de tiempo desde la primera vez de solicitud para almacenar en caché el contenido. 

Ejemplo:

```cshtml
<Cache expires-on="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a>deslizando expira

| Tipo de atributo    | Valor de ejemplo     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(60)"     |


Establece el tiempo que se debe expulsar una entrada de caché si no se ha accedido.

Ejemplo:

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a>por encabezado Vary

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| Cadena            | "User-Agent"                  |
|                   | "User-Agent, content-encoding" |

Acepta un valor de encabezado único o una lista separada por comas de los valores de encabezado que desencadenan una actualización de la caché cuando cambian. En el ejemplo siguiente se supervisa el valor del encabezado `User-Agent`. En el ejemplo se almacenará en memoria caché el contenido de cada diferentes `User-Agent` muestre en el servidor web.

Ejemplo:

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a>variar por consulta

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| Cadena            | "Hacer"                |
|                   | "Asegúrese, modelo" |

Acepta un valor de encabezado único o una lista separada por comas de los valores de encabezado que desencadenan una actualización de la caché cuando cambia el valor de encabezado. En el ejemplo siguiente se busca en los valores de `Make` y `Model`.

Ejemplo:

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a>variar ruta

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| Cadena            | "Hacer"                |
|                   | "Asegúrese, modelo" |

Acepta un valor de encabezado único o una lista separada por comas de los valores de encabezado que desencadenan una actualización de la caché cuando cambio de valores de parámetro de ruta de datos. Ejemplo:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a>variar cookie

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| Cadena            | ". AspNetCore.Identity.Application"                |
|                   | ". AspNetCore.Identity.Application,HairColor" |

Acepta un valor de encabezado único o una lista separada por comas de los valores de encabezado que desencadenan una actualización de la caché cuando los valores de encabezado cambio (s). En el ejemplo siguiente se examina la cookie asociada con la identidad de ASP.NET. Cuando un usuario se autentica la cookie de solicitud para establecer lo que desencadena una actualización de la caché.

Ejemplo:

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a>variar por usuario

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| Booleano             | "true"                  |
|                     | "false" (predeterminado) |

Especifica si debe restablecer la memoria caché cuando el usuario ha iniciado la sesión (o la entidad de contexto) cambia. El usuario actual es también conocida como la entidad de contexto de solicitud y puede verse en una vista Razor haciendo referencia a `@User.Identity.Name`.

En el ejemplo siguiente se examina el usuario conectado actualmente.  

Ejemplo:

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

Con este atributo, mantiene el contenido en caché a través de un ciclo de inicio de sesión y registro de salida.  Al utilizar `vary-by-user="true"`, una acción de inicio de sesión y registro horizontal invalida la caché para el usuario autenticado.  Se invalida la memoria caché porque se genera un nuevo valor de cookie único inicio de sesión. Memoria caché se mantiene para el estado anónimo cuando ninguna cookie está presente o ha expirado. Esto significa que si ningún usuario ha iniciado sesión, se mantendrá la memoria caché.

- - -

### <a name="vary-by"></a>variar por

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| Cadena             | "@Model"                 |


Permite la personalización de los datos que se almacena en caché. Cuando se actualiza el objeto al que hace referencia cambia de valor de cadena del atributo, el contenido de la aplicación auxiliar de etiqueta de caché. A menudo una concatenación de cadenas de valores del modelo se asignan a este atributo.  De hecho, que significa que una actualización a cualquiera de los valores concatenados invalida la memoria caché.

El ejemplo siguiente supone que el método de controlador representar el valor del entero de los dos parámetros de ruta, de las sumas de vista `myParam1` y `myParam2`y que devuelve como la propiedad de modelo simple. Cuando se cambia esta suma, el contenido de la aplicación auxiliar de etiqueta de caché se representan y almacenado en caché nuevo.  

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
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a>priority

| Tipo de atributo    | Valores de ejemplo                |
|----------------   |----------------               |
| Enumeración CacheItemPriority  | "Alto"                   |
|                    | "Bajo" |
|                    | "NeverRemove" |
|                    | "Normal" |

Proporciona instrucciones de expulsión de caché para el proveedor de caché integrada. El servidor web expulsará `Low` entradas de caché de primero cuando está bajo presión de memoria.

Ejemplo:

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

El `priority` atributos no garantizan un nivel específico de retención de la memoria caché. `CacheItemPriority`es sólo una sugerencia. Establecer este atributo en `NeverRemove` no garantiza que siempre se conservará la memoria caché. Vea [recursos adicionales](#additional-resources) para obtener más información.

La aplicación auxiliar de etiqueta de caché es dependiente de la [servicio de caché de memoria](xref:performance/caching/memory). La aplicación auxiliar de etiqueta de caché agrega el servicio si no se ha agregado.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
