---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Enrutamiento y selección de acción en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Enrutamiento y selección de acción en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artículo describe cómo ASP.NET Web API enruta una solicitud HTTP para una acción concreta en un controlador.

> [!NOTE]
> Para obtener una visión general de enrutamiento, consulte [enrutamiento de ASP.NET Web API](routing-in-aspnet-web-api.md).


En este artículo se examina los detalles del proceso de enrutamiento. Si se crea un proyecto de Web API y búsqueda que no obtienen algunas solicitudes enruta la forma que espera, espero que este artículo le ayudará.

Enrutamiento consta de tres fases principales:

1. Coincidir con el URI para una plantilla de ruta.
2. Al seleccionar un controlador.
3. Selecciona la acción.

Puede reemplazar algunas partes del proceso con sus propios comportamientos personalizados. En este artículo, describe el comportamiento predeterminado. Al final, tenga en cuenta los lugares donde puede personalizar el comportamiento.

## <a name="route-templates"></a>Plantillas de ruta

Una plantilla de ruta es similar a una ruta de acceso URI, pero puede tener valores de marcador de posición, indicados con llaves:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Cuando se crea una ruta, puede proporcionar valores predeterminados para algunos o todos los marcadores de posición:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

También puede proporcionar restricciones, que le limitan cómo un segmento de URI puede coincidir con un marcador de posición:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

El marco de trabajo intenta hacer coincidir los segmentos en la ruta de acceso URI a la plantilla. Literales en la plantilla deben coincidir exactamente. Un marcador de posición coincide con algún valor, a menos que especifique las restricciones. El marco de trabajo no coincide con otras partes del identificador URI, como el nombre de host o los parámetros de consulta. El marco de trabajo, selecciona la primera ruta en la tabla de ruta que coincide con el URI.

Hay dos marcadores de posición especial: "{controller}" y "{action}".

- "{controller}" proporciona el nombre del controlador.
- "{action}" proporciona el nombre de la acción. En la API de Web, la convención habitual es omitir "{action}".

### <a name="defaults"></a>del parte de horas

Si proporciona valores predeterminados, la ruta se corresponderá con un URI que le falta esos segmentos. Por ejemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

El URI "`http://localhost/api/products`" coincide con esta ruta. El segmento "{categoría}" se asigna el valor predeterminado "all".

### <a name="route-dictionary"></a>Diccionario de ruta

Si el marco de trabajo encuentra a una coincidencia para un URI, crea un diccionario que contiene el valor para cada marcador de posición. Las claves son los nombres de marcador de posición, sin incluir las llaves. Los valores se toman de la ruta de acceso URI o de los valores predeterminados. El diccionario se almacena en la **IHttpRouteData** objeto.

Durante esta fase de coincidencia de ruta, el especial "{controller}" y los marcadores de posición "{action}" se tratan igual que los otros marcadores de posición. Simplemente se almacenan en el diccionario con los demás valores.

Valor predeterminado puede tener el valor especial **RouteParameter.Optional**. Si un marcador de posición se asigna este valor, el valor no se agrega al diccionario de ruta. Por ejemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Para la ruta de acceso URI "api/products", el diccionario de ruta contendrá:

- controlador: "productos"
- categoría: "todo"

Para "api/productos/toys/123", sin embargo, el diccionario de ruta contendrá:

- controlador: "productos"
- categoría: "toys"
- Id.: "123"

Los valores predeterminados también pueden incluir un valor que no aparece en cualquier lugar en la plantilla de ruta. Si la ruta coincide con, ese valor se almacena en el diccionario. Por ejemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Si la ruta de acceso URI es "api/raíz/8", el diccionario contiene dos valores:

- controlador: "customers"
- Id.: "8"

## <a name="selecting-a-controller"></a>Al seleccionar un controlador

Selección del controlador se controla mediante la **IHttpControllerSelector.SelectController** método. Este método toma una **HttpRequestMessage** instancia y devuelve una **HttpControllerDescriptor**. Proporciona la implementación predeterminada del **DefaultHttpControllerSelector** clase. Esta clase utiliza un algoritmo sencillo:

1. Busque en el diccionario de ruta para la clave "controller".
2. Tomar el valor de esta clave y agregar la cadena "Controller" para obtener el nombre del tipo de controlador.
3. Busque un controlador de API Web con este nombre de tipo.

Por ejemplo, si el diccionario de ruta contiene el par de clave y valor "controlador" "="productos", el tipo de controlador es"ProductsController". Si no hay ningún tipo de coincidencia o varias coincidencias, el marco de trabajo devuelve un error al cliente.

En el paso 3, **DefaultHttpControllerSelector** utiliza la **IHttpControllerTypeResolver** interfaz para obtener una lista de los tipos de controlador Web API. La implementación predeterminada de **IHttpControllerTypeResolver** devuelve todas las clases públicas que implementan (a) **IHttpController**, (b) son no abstracta y (c) tiene un nombre que termina en "Controller".

## <a name="action-selection"></a>Selección de acción

Después de seleccionar el controlador, el marco de trabajo selecciona la acción mediante una llamada a la **IHttpActionSelector.SelectAction** método. Este método toma una **HttpControllerContext** y devuelve un **HttpActionDescriptor**.

Proporciona la implementación predeterminada del **ApiControllerActionSelector** clase. Para seleccionar una acción, examina el siguiente:

- El método HTTP de la solicitud.
- El marcador de posición "{action}" en la plantilla de ruta, si está presente.
- Los parámetros de las acciones en el controlador.

Antes de buscar en el algoritmo de selección, es necesario comprender algunas cosas acerca de las acciones de controlador.

**¿Los métodos en el controlador se consideran "acciones"?** Al seleccionar una acción, el marco de trabajo solo se examina en métodos de instancia pública en el controlador. Además, excluye ["nombre especial"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) métodos (constructores, eventos, sobrecargas de operador etc.) y métodos heredados de la **ApiController** clase.

**Métodos HTTP.** El marco de trabajo solo elige acciones que coinciden con el método HTTP de la solicitud, que se determina como sigue:

1. Puede especificar el método HTTP con un atributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, o **HttpPut**.
2. En caso contrario, si el nombre del método de controlador empieza por "Get", "Post", "Put", "Delete", "Head", "Opciones" o "Revisión", a continuación, por convención la acción es compatible con ese método HTTP.
3. Si ninguno de los pasos anteriores, el método es compatible con POST.

**Enlaces de parámetros.** Un enlace de parámetro es la API Web crea un valor para un parámetro. Aquí es la regla predeterminada para el enlace de parámetros:

- Tipos simples se toman del URI.
- Tipos complejos se realizan desde el cuerpo de solicitud.

Los tipos simples incluyen todos los [tipos primitivos de .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), además de **DateTime**, **Decimal**, **Guid**, **cadena** , y **TimeSpan**. Por cada acción, a lo sumo un parámetro puede leer el cuerpo de solicitud.

> [!NOTE]
> Es posible reemplazar las reglas de enlace predeterminadas. Vea [enlace de parámetros de WebAPI bajo el paraguas](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Con ese fondo, aquí es el algoritmo de selección de acción.

1. Crear una lista de todas las acciones en el controlador que coincide con el método de solicitud HTTP.
2. Si el diccionario de ruta tenga una entrada de "acción", quitar las acciones cuyo nombre no coincide con este valor.
3. Intenta coincidir con los parámetros de acción para el identificador URI, como se indica a continuación: 

    1. Para cada acción, obtener una lista de los parámetros que son un tipo simple, donde el enlace obtiene el parámetro del identificador URI. Excluya los parámetros opcionales.
    2. En esta lista, intenta buscar a una coincidencia para cada nombre de parámetro, en el diccionario de ruta o en la cadena de consulta URI. Las coincidencias no distinguen mayúsculas de minúsculas y no dependen del orden de los parámetros.
    3. Seleccione una acción en cada parámetro de la lista tiene una coincidencia en el URI.
    4. Si hay más que una acción cumple estos criterios, elija una con la mayor parte coincide con el parámetro.
4. Pasar por alto las acciones con el **[NonAction]** atributo.

Paso 3 de # es probablemente la más confuso. La idea básica es que un parámetro puede obtener su valor del URI, desde el cuerpo de la solicitud o desde un enlace personalizado. Para los parámetros que proceden de lo URI, debe asegurarse de que el URI contiene un valor para ese parámetro, en la ruta de acceso (mediante el diccionario de ruta) o en la cadena de consulta.

Por ejemplo, tenga en cuenta la acción siguiente:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

El *identificador* parámetro se enlaza a la dirección URI. Por lo tanto, esta acción solo puede coincidir con un URI que contiene un valor para "id", en el diccionario de ruta o en la cadena de consulta.

Parámetros opcionales son una excepción, ya que son opcionales. Para un parámetro opcional, es apropiado si el enlace no puede obtener el valor del identificador URI.

Los tipos complejos son una excepción por un motivo distinto. Un tipo complejo solamente puede enlazar al identificador URI a través de un enlace personalizado. Pero en ese caso, no se puede conocer el marco de trabajo de antemano si el parámetro enlazaría a un URI determinado. Para obtener información, que sería necesario invocar el enlace. Es el objetivo del algoritmo de selección seleccionar una acción de la descripción estática, antes de invocar los enlaces. Por lo tanto, se excluyen tipos complejos en el algoritmo de coincidencia.

Después de selecciona la acción, se invocan todos los enlaces de parámetro.

Resumen:

- La acción debe coincidir con el método HTTP de la solicitud.
- El nombre de acción debe coincidir con la entrada de "acción" en el diccionario de ruta, si está presente.
- Para cada parámetro de la acción, si el parámetro procede de la dirección URI, a continuación, el nombre del parámetro debe encontrarse en el diccionario de ruta o en la cadena de consulta URI. (Se excluyen los parámetros opcionales y los parámetros con tipos complejos.)
- Intentar hacer coincidir el mayor número de parámetros. La mejor coincidencia podría ser un método sin parámetros.

## <a name="extended-example"></a>Ejemplo completo

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

El diccionario de ruta no contiene los parámetros de cadena de consulta, "versión" y "Detalles", pero éstos se seguirá considerando durante la selección de acción.

### <a name="controller-selection"></a>Selección de controlador

Desde la entrada "controller" en el diccionario de ruta, el tipo de controlador es `ProductsController`.

### <a name="action-selection"></a>Selección de acción

La solicitud HTTP es una solicitud GET. Las acciones de controlador que admiten GET están `GetAll`, `GetById`, y `FindProductsByName`. El diccionario de ruta no contiene una entrada para "acción", por lo que no es necesario para que coincida con el nombre de acción.

A continuación, se intenta hacer coincidir los nombres de parámetro para las acciones, mirar solo según las acciones de GET.

| Acción | Parámetros de coincidencia |
| --- | --- |
| `GetAll` | ninguna |
| `GetById` | "ID". |
| `FindProductsByName` | "name" |

Tenga en cuenta que la *versión* parámetro de `GetById` no se considera, porque es un parámetro opcional.

El `GetAll` método coincide con trivial. El `GetById` método también coincide con, porque el diccionario de ruta contiene "id". El `FindProductsByName` no coincide con el método.

El `GetById` método wins, dado que coincide con un parámetro, frente a ningún parámetro para `GetAll`. El método se invoca con los siguientes valores de parámetro:

- *id* = 1
- *versión* = 1.5

Tenga en cuenta que aunque *versión* no se utiliza en el algoritmo de selección, el valor del parámetro procede de la cadena de consulta URI.

## <a name="extension-points"></a>Puntos de extensión

API Web proporciona puntos de extensión para algunas partes del proceso de enrutamiento.

| Interfaz | Descripción |
| --- | --- |
| **IHttpControllerSelector** | Selecciona el controlador. |
| **IHttpControllerTypeResolver** | Obtiene la lista de los tipos de controlador. El **DefaultHttpControllerSelector** elige el tipo de controlador de esta lista. |
| **IAssembliesResolver** | Obtiene la lista de ensamblados del proyecto. El **IHttpControllerTypeResolver** interfaz usa esta lista para encontrar los tipos de controlador. |
| **IHttpControllerActivator** | Crea nuevas instancias de controlador. |
| **IHttpActionSelector** | Selecciona la acción. |
| **IHttpActionInvoker** | Invoca la acción. |

Para proporcionar su propia implementación de cualquiera de estas interfaces, utilice la **servicios** colección en la **HttpConfiguration** objeto:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
