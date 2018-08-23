---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Enrutamiento y selección de acción en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/27/2012
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: b4912d3ee1e13651f2a63d54d7dbfd92e00f85f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836121"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Enrutamiento y selección de acción en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describe cómo ASP.NET Web API enruta una solicitud HTTP a una acción determinada en un controlador.

> [!NOTE]
> Para obtener una visión general del enrutamiento, consulte [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).


Este artículo examina los detalles del proceso de enrutamiento. Si crea un proyecto Web API y búsqueda que no hagan algunas solicitudes enruta según que lo previsto, espero que este artículo le ayude.

Enrutamiento tiene tres fases principales:

1. Coincide con el URI para una plantilla de ruta.
2. Al seleccionar un controlador.
3. Seleccione una acción.

Puede reemplazar algunas partes del proceso con sus propios comportamientos personalizados. En este artículo, describiré el comportamiento predeterminado. Al final, tenga en cuenta los lugares donde puede personalizar el comportamiento.

## <a name="route-templates"></a>Plantillas de ruta

Una plantilla de ruta es similar a una ruta de acceso URI, pero puede tener valores de marcador de posición que se indica con llaves:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Cuando se crea una ruta, puede proporcionar valores predeterminados para algunos o todos los marcadores de posición:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

También puede proporcionar restricciones, que limiten cómo un segmento de URI puede coincidir con un marcador de posición:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

El marco de trabajo intenta hacer coincidir los segmentos en la ruta de acceso URI a la plantilla. En la plantilla literales deben coincidir exactamente. Un marcador de posición coincide con algún valor, a menos que especifique las restricciones. El marco de trabajo no coincide con otras partes del URI, como el nombre de host o los parámetros de consulta. El marco de trabajo, selecciona la primera ruta en la tabla de ruta que coincida con el identificador URI.

Hay dos marcadores de posición especiales: "{controller}" y "{action}".

- "{controller}" proporciona el nombre del controlador.
- "{action}" proporciona el nombre de la acción. En la API Web, la convención habitual es omitir "{action}".

### <a name="defaults"></a>del parte de horas

Si proporciona valores predeterminados, la ruta se corresponderá con un URI que le falta esos segmentos. Por ejemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

El URI "`http://localhost/api/products`" coincide con esta ruta. El segmento "{category}" se asigna el valor predeterminado "all".

### <a name="route-dictionary"></a>Diccionario de ruta

Si el marco de trabajo encuentra a una coincidencia para un URI, crea un diccionario que contiene el valor para cada marcador de posición. Las claves son los nombres de marcador de posición, no se incluye entre llaves. Los valores se toman de la ruta de acceso URI o de los valores predeterminados. El diccionario se almacena en el **IHttpRouteData** objeto.

Durante esta fase de coincidencia de ruta, el especial "{controller}" y los marcadores de posición "{action}" se tratan igual que los otros marcadores de posición. Simplemente se almacenan en el diccionario con los demás valores.

Un valor predeterminado puede tener el valor especial **RouteParameter.Optional**. Si un marcador de posición se asigna este valor, el valor no se agrega al diccionario de ruta. Por ejemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Para la ruta de acceso URI "api/products", el diccionario de ruta contendrá:

- controlador: "productos"
- categoría: "all"

Para "api/productos/toys/123", sin embargo, el diccionario de ruta contendrá:

- controlador: "productos"
- categoría: "toys"
- Id.: "123"

Los valores predeterminados también pueden incluir un valor que no aparece en la plantilla de ruta. Si coincide con la ruta, ese valor se almacena en el diccionario. Por ejemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Si la ruta de acceso URI es "api/raíz/8", el diccionario contendrá dos valores:

- controlador: "clientes"
- Id.: "8"

## <a name="selecting-a-controller"></a>Al seleccionar un controlador

Selección del controlador se controla mediante el **IHttpControllerSelector.SelectController** método. Este método toma un **HttpRequestMessage** instancia y devuelve un **HttpControllerDescriptor**. Proporciona la implementación predeterminada del **DefaultHttpControllerSelector** clase. Esta clase utiliza un algoritmo sencillo:

1. Busque en el diccionario de ruta para la clave "controller".
2. Tomamos el valor de esta clave y anexe la cadena "Controller" para obtener el nombre del tipo de controlador.
3. Busque un controlador Web API con este nombre de tipo.

Por ejemplo, si el diccionario de ruta contiene el par clave-valor "controlador" = "productos", el tipo de controlador es "ProductsController". Si no hay ningún tipo de coincidencia o varias coincidencias, el marco de trabajo devuelve un error al cliente.

Para el paso 3, **DefaultHttpControllerSelector** usa el **IHttpControllerTypeResolver** interfaz para obtener la lista de tipos de controlador de API Web. La implementación predeterminada de **IHttpControllerTypeResolver** devuelve todas las clases públicas que implementan (a) **IHttpController**, (b) son no abstracta y (c) tiene un nombre que termina en "Controller".

## <a name="action-selection"></a>Selección de acción

Después de seleccionar el controlador, framework selecciona la acción mediante una llamada a la **IHttpActionSelector.SelectAction** método. Este método toma un **HttpControllerContext** y devuelve un **HttpActionDescriptor**.

Proporciona la implementación predeterminada del **ApiControllerActionSelector** clase. Para seleccionar una acción, es similar al siguiente:

- El método HTTP de la solicitud.
- El marcador de posición "{action}" en la plantilla de ruta, si está presente.
- Los parámetros de las acciones en el controlador.

Antes de ver el algoritmo de selección, es necesario comprender algunas cosas sobre las acciones de controlador.

**¿Los métodos en el controlador se consideran "acciones"?** Al seleccionar una acción, el marco de trabajo busca solo en métodos de instancia públicos en el controlador. Además, excluye ["nombre especial"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) métodos (constructores, eventos, las sobrecargas del operador etc.) y métodos heredados de la **ApiController** clase.

**Métodos HTTP.** El marco de trabajo solo elige las acciones que coinciden con el método HTTP de la solicitud, se determina como sigue:

1. Puede especificar el método HTTP con un atributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, o **HttpPut**.
2. En caso contrario, si el nombre del método controller comienza con "Get", "Post", "Put", "Delete", "Head", "Opciones" o "Revisión", a continuación, por convención, la acción es compatible con ese método HTTP.
3. Si ninguno de los anteriores, el método admite la publicación.

**Enlaces de parámetros.** Un enlace de parámetros es cómo la API Web crea un valor para un parámetro. Aquí es la regla predeterminada para el enlace de parámetros:

- Tipos simples se toman de la dirección URI.
- Tipos complejos se toman del cuerpo de la solicitud.

Los tipos simples incluyen todos los [tipos primitivos de .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), además de **DateTime**, **Decimal**, **Guid**, **cadena** , y **TimeSpan**. Para cada acción, como máximo un parámetro puede leer el cuerpo de solicitud.

> [!NOTE]
> Es posible invalidar las reglas de enlace predeterminada. Consulte [enlace de parámetros de WebAPI debajo del capó](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Con esa información, aquí es el algoritmo de selección de acción.

1. Crear una lista de todas las acciones en el controlador que coincida con el método de solicitud HTTP.
2. Si el diccionario de ruta tiene una entrada de "acción", quite las acciones cuyo nombre no coincide con este valor.
3. Intenta coincidir con los parámetros de acción al URI, como se indica a continuación: 

    1. Para cada acción, obtenga una lista de los parámetros que son un tipo simple, donde el enlace obtiene el parámetro del URI. Excluir los parámetros opcionales.
    2. En esta lista, intenta encontrar a una coincidencia para cada nombre de parámetro, en el diccionario de ruta o en la cadena de consulta URI. Las coincidencias no distinguen mayúsculas de minúsculas y no dependen de la orden de los parámetros.
    3. Seleccione una acción donde todos los parámetros de la lista tiene una coincidencia en el URI.
    4. Si más que una acción cumple estos criterios, elija uno con la mayoría coincidencias de parámetro.
4. Pasar por alto las acciones con el **[NonAction]** atributo.

Paso #3 es probablemente el más confuso. La idea básica es que un parámetro puede obtener su valor desde el URI, desde el cuerpo de solicitud o desde un enlace personalizado. Para los parámetros que proceden de la dirección URI, queremos asegurarnos de que el URI contiene realmente un valor para ese parámetro, en la ruta de acceso (mediante el diccionario de ruta) o en la cadena de consulta.

Por ejemplo, considere la siguiente acción:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

El *id* parámetro se enlaza a la dirección URI. Por lo tanto, esta acción solo puede coincidir con un URI que contiene un valor para "id", en el diccionario de ruta o en la cadena de consulta.

Los parámetros opcionales son una excepción, porque son opcionales. Para un parámetro opcional, es correcto si el enlace no puede obtener el valor del URI.

Los tipos complejos son una excepción por un motivo distinto. Un tipo complejo sólo puede enlazar al identificador URI a través de un enlace personalizado. Pero en ese caso, no se puede saber el marco de trabajo de antemano si el parámetro se enlazaría a un URI determinado. Para obtener, deberá invocar el enlace. Es el objetivo del algoritmo de selección seleccionar una acción de la descripción estática, antes de invocar todos los enlaces. Por lo tanto, los tipos complejos se excluyen el algoritmo de coincidencia.

Después de selecciona la acción, se invocan todos los enlaces de parámetro.

Resumen:

- La acción debe coincidir con el método HTTP de la solicitud.
- El nombre de acción debe coincidir con la entrada de "acción" en el diccionario de ruta, si está presente.
- Para cada parámetro de la acción, si se toma el parámetro de la dirección URI, a continuación, el nombre del parámetro se debe encontrar en el diccionario de ruta o en la cadena de consulta URI. (Se excluyen los parámetros opcionales y los parámetros con tipos complejos.)
- Intenta coincidir con el mayor número de parámetros. La mejor coincidencia podría ser un método sin parámetros.

## <a name="extended-example"></a>Ejemplo ampliado

Rutas:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Controlador:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Solicitud HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Coincidencia de rutas

El URI coincida con la ruta denominada "DefaultApi". El diccionario de ruta contiene las siguientes entradas:

- controlador: "productos"
- Id.: "1"

El diccionario de ruta no contiene los parámetros de cadena de consulta, "version" y "Detalles", pero estos se seguirá considerando durante la selección de acción.

### <a name="controller-selection"></a>Selección de controlador

En la entrada "controller" en el diccionario de ruta, el tipo de controlador es `ProductsController`.

### <a name="action-selection"></a>Selección de acción

La solicitud HTTP es una solicitud GET. Las acciones de controlador que se admiten GET son `GetAll`, `GetById`, y `FindProductsByName`. El diccionario de ruta no contiene una entrada para "action", por lo que no necesitamos para que coincida con el nombre de acción.

A continuación, intentamos hacer coincidir los nombres de parámetro para las acciones, mirar solo según las acciones GET.

| Acción | Parámetros de coincidencia |
| --- | --- |
| `GetAll` | ninguna |
| `GetById` | "ID". |
| `FindProductsByName` | "name" |

Tenga en cuenta que el *versión* parámetro de `GetById` no se considera, porque es un parámetro opcional.

El `GetAll` método coincide con forma trivial. El `GetById` método también coincide con, ya que el diccionario de ruta contiene "id". El `FindProductsByName` no coincide con el método.

El `GetById` método wins, dado que coincide con un parámetro, frente a sin parámetros para `GetAll`. Se invoca el método con los siguientes valores de parámetro:

- *Id. de* = 1
- *versión* = 1.5

Tenga en cuenta que aunque *versión* no se ha usado en el algoritmo de selección, el valor del parámetro procede de la cadena de consulta URI.

## <a name="extension-points"></a>Puntos de extensión

API Web proporciona puntos de extensión para algunas partes del proceso de enrutamiento.

| Interfaz | Descripción |
| --- | --- |
| **IHttpControllerSelector** | Selecciona el controlador. |
| **IHttpControllerTypeResolver** | Obtiene la lista de tipos de controlador. El **DefaultHttpControllerSelector** elige el tipo de controlador de esta lista. |
| **IAssembliesResolver** | Obtiene la lista de ensamblados de proyecto. El **IHttpControllerTypeResolver** interfaz utiliza esta lista para buscar los tipos de controlador. |
| **IHttpControllerActivator** | Crea nuevas instancias de controlador. |
| **IHttpActionSelector** | Selecciona la acción. |
| **IHttpActionInvoker** | Invoca la acción. |

Para proporcionar su propia implementación de cualquiera de estas interfaces, utilice el **servicios** colección en el **HttpConfiguration** objeto:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
