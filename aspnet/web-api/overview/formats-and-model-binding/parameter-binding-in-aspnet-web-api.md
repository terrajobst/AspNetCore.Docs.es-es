---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parámetro de enlace en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97785db962691c25bac03f904924321af2f6b500
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810087"
---
<a name="parameter-binding-in-aspnet-web-api"></a>Parámetro de enlace en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Cuando la API Web llama a un método en un controlador, se deben establecer valores para los parámetros, un proceso denominado *enlace*. En este artículo se describe cómo Web API enlaza los parámetros y cómo puede personalizar el proceso de enlace.

De forma predeterminada, Web API usa las siguientes reglas para enlazar parámetros:

- Si el parámetro es un tipo "simple", API Web intenta obtener el valor del URI. Los tipos simples incluyen .NET [tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **doble**, y así sucesivamente), además de **TimeSpan**, **DateTime**, **Guid**, **decimal**, y **cadena**, *plus* cualquier tipo con un convertidor de tipos puede convertir una cadena. (Más información acerca de los convertidores de tipos más adelante).
- Para tipos complejos, API Web intenta leer el valor del cuerpo del mensaje, pero utiliza un [formateador de tipo de medio](media-formatters.md).

Por ejemplo, este es un método de controlador Web API típico:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

El *id* parámetro es un &quot;simple&quot; escriba, por lo que la API Web intenta obtener el valor URI de la solicitud. El *elemento* parámetro es un tipo complejo, por lo que la API Web utiliza un formateador de tipo de medio para leer el valor desde el cuerpo de solicitud.

Para obtener un valor de identificador URI, Web API se busca en los datos de ruta y la cadena de consulta URI. Los datos de ruta se rellenan cuando el sistema de enrutamiento analiza el identificador URI y coincide con una ruta. Para obtener más información, consulte [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md).

En el resto de este artículo, le mostraré cómo puede personalizar el proceso de enlace de modelo. Para los tipos complejos, sin embargo, considere el uso de formateadores de tipo multimedia siempre que sea posible. Un principio clave de HTTP es que los recursos se envían en el cuerpo del mensaje utilizando la negociación de contenido para especificar la representación del recurso. Formateadores de tipo de medio se diseñaron exactamente para este propósito.

## <a name="using-fromuri"></a>Uso de [FromUri]

Para forzar a Web API para leer un tipo complejo del URI, agregue el **[FromUri]** al parámetro. En el ejemplo siguiente se define un `GeoPoint` tipo, junto con un método de controlador que obtiene el `GeoPoint` desde el URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

El cliente puede colocar los valores de latitud y longitud en la cadena de consulta y Web API se usarán para construir un `GeoPoint`. Por ejemplo:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Uso de [FromBody]

Para forzar a Web API para leer un tipo simple desde el cuerpo de solicitud, agregue el **[FromBody]** al parámetro:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

En este ejemplo, API Web usará un formateador de tipo de medio para leer el valor de *nombre* desde el cuerpo de solicitud. Este es un ejemplo de solicitud de cliente.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Cuando un parámetro tiene [FromBody], API Web usa el encabezado Content-Type para seleccionar a un formateador. En este ejemplo, el tipo de contenido es &quot;application/json&quot; y el cuerpo de solicitud es una cadena JSON sin formato (no un objeto JSON).

A lo sumo un parámetro puede leer desde el cuerpo del mensaje. Por lo que esto no funcionará:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

La razón de esta regla es que el cuerpo de solicitud podría almacenarse en una secuencia sin búfer que solo se puede leer una vez.

## <a name="type-converters"></a>Convertidores de tipos

Puede realizar Web API tratar una clase como un tipo simple (para que Web API intentará enlazar desde el URI) mediante la creación de un **TypeConverter** y proporcionando una conversión de cadena.

El código siguiente muestra un `GeoPoint` clase que representa un punto geográfico, más una **TypeConverter** que convierte las cadenas a `GeoPoint` instancias. El `GeoPoint` clase se decora con un **[TypeConverter]** atributo para especificar el convertidor de tipos. (En este ejemplo se inspiró en la entrada de blog de Mike Stall [cómo enlazar a objetos personalizados en las firmas de acción de MVC y WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Ahora se tratará la API Web `GeoPoint` como un tipo simple, lo que significa que intentará enlazar `GeoPoint` parámetros desde el URI. No es necesario incluir **[FromUri]** en el parámetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

El cliente puede invocar el método con un URI como este:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Enlazadores modelo

Es una opción más flexible que un convertidor de tipos crear un enlazador de modelos personalizado. Con un enlazador de modelos, tendrá acceso a cosas como la solicitud HTTP, la descripción de la acción y los valores sin procesar de los datos de ruta.

Para crear un enlazador de modelos, implemente el **IModelBinder** interfaz. Esta interfaz define un método único, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Este es un enlazador de modelos para `GeoPoint` objetos.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Un enlazador de modelos obtiene los valores de entrada sin procesar de un *proveedor de valores*. Este diseño separa dos funciones distintas:

- El proveedor de valores recibe la solicitud HTTP y rellena un diccionario de pares clave-valor.
- El enlazador de modelos usa este diccionario para rellenar el modelo.

El proveedor de valor predeterminado en la API Web obtiene valores de los datos de ruta y la cadena de consulta. Por ejemplo, si el URI es `http://localhost/api/values/1?location=48,-122`, el proveedor de valores crea los siguientes pares clave-valor:

- Id. = &quot;1&quot;
- ubicación = &quot;48,122&quot;

(Supongo que la plantilla de ruta predeterminada, que es &quot;api / {controller} / {id}&quot;.)

El nombre del parámetro que se va a enlazar se almacena en el **ModelBindingContext.ModelName** propiedad. El enlazador de modelos busca una clave con este valor en el diccionario. Si el valor existe y se puede convertir en un `GeoPoint`, el enlazador de modelos asigna el valor enlazado a la **ModelBindingContext.Model** propiedad.

Tenga en cuenta que el enlazador de modelos no se limita a una conversión de tipo simple. En este ejemplo, el enlazador de modelos busca primero en una tabla de ubicaciones conocidas y, si se produce un error, utiliza la conversión de tipo.

**Establecer el enlazador de modelos**

Hay varias maneras de establecer un enlazador de modelos. En primer lugar, puede agregar un **[ModelBinder]** al parámetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

También puede agregar un **[ModelBinder]** al tipo de atributo. API Web usará el enlazador de modelos especificado para todos los parámetros de ese tipo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Por último, puede agregar un proveedor de enlazador de modelos para la **HttpConfiguration**. Un proveedor de enlazadores de modelos es simplemente una clase de generador que crea un enlazador de modelos. Puede crear un proveedor mediante la derivación de la [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) clase. Sin embargo, si su enlazador de modelos administra un solo tipo, resulta más fácil de usar integrado **SimpleModelBinderProvider**, que está diseñado para este propósito. El código siguiente muestra cómo hacerlo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Con un proveedor de enlace de modelos, deberá agregar el **[ModelBinder]** al parámetro, para indicar a la API Web que debe usar un enlazador de modelos y no un formateador de tipo de medio. Pero ahora no es necesario especificar el tipo de enlazador de modelos en el atributo:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Proveedores de valor

Mencioné que un enlazador de modelos obtiene los valores de un proveedor de valores. Para escribir un proveedor de valores personalizados, implementar la **IValueProvider** interfaz. Este es un ejemplo que extrae los valores de las cookies en la solicitud:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

También deberá crear una fábrica de proveedor de valor mediante la derivación desde el **ValueProviderFactory** clase.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Agregar el generador de proveedores de valor para el **HttpConfiguration** como sigue.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

API Web compone de todos los proveedores de valor, por lo que cuando llama un enlazador de modelos **ValueProvider.GetValue**, el enlazador de modelos recibe el valor desde el primer proveedor de valor que es capaz de producirlo.

Como alternativa, puede establecer el generador de proveedores de valor en el nivel de parámetro mediante el **ValueProvider** atributo, como se indica a continuación:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Esto indica a Web API para usar el enlace de modelos con el generador de proveedores de valor especificado y no se debe utilizar cualquiera de los otros proveedores de valor registrados.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Los enlazadores de modelos son una instancia específica de un mecanismo más general. Si observa la **[ModelBinder]** atributo, verá que se deriva de la clase abstracta **ParameterBindingAttribute** clase. Esta clase define un método único, **GetBinding**, que devuelve un **HttpParameterBinding** objeto:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Un **HttpParameterBinding** es responsable de enlazar un parámetro a un valor. En el caso de **[ModelBinder]**, devuelve el atributo una **HttpParameterBinding** implementación que utiliza un **IModelBinder** para realizar el enlace real. También puede implementar su propio **HttpParameterBinding**.

Por ejemplo, suponga que desea obtener ETags desde `if-match` y `if-none-match` encabezados en la solicitud. Comenzaremos por definir una clase para representar valores de ETag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

También definiremos una enumeración para indicar si se debe obtener el valor ETag de la `if-match` encabezado o el `if-none-match` encabezado.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Este es un **HttpParameterBinding** que obtiene el valor ETag del encabezado deseado y lo enlaza a un parámetro de tipo ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

El **ExecuteBindingAsync** método realiza el enlace. Dentro de este método, agregue el valor del parámetro enlazado a la **ActionArgument** diccionario en el **HttpActionContext**.

> [!NOTE]
> Si su **ExecuteBindingAsync** método lee el cuerpo del mensaje de solicitud, reemplace el **WillReadBody** propiedad devuelva true. El cuerpo de solicitud podría ser una secuencia sin almacenamiento en búfer que solo se puede leer una vez, por lo que la API Web se aplica una regla que a lo sumo un enlace puede leer el cuerpo del mensaje.


Para aplicar un personalizado **HttpParameterBinding**, puede definir un atributo que se deriva de **ParameterBindingAttribute**. Para `ETagParameterBinding`, definiremos dos atributos, uno para `if-match` encabezados y otra para `if-none-match` encabezados. Ambos derivan de una clase base abstracta.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Este es un método de controlador que utiliza el `[IfNoneMatch]` atributo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Además **ParameterBindingAttribute**, hay otro enlace para agregar un personalizado **HttpParameterBinding**. En el **HttpConfiguration** objeto, el **ParameterBindingRules** propiedad es una colección de funciones anomymous de tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Por ejemplo, podría agregar una regla que utiliza cualquier otro parámetro ETag en un método GET `ETagParameterBinding` con `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

La función debe devolver `null` para los parámetros donde el enlace no es aplicable.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Todo el proceso de enlace de parámetros se controla mediante un servicio conectable, **IActionValueBinder**. La implementación predeterminada de **IActionValueBinder** hace lo siguiente:

1. Busque un **ParameterBindingAttribute** en el parámetro. Esto incluye **[FromBody]**, **[FromUri]**, y **[ModelBinder]**, o los atributos personalizados.
2. En caso contrario, busque en **HttpConfiguration.ParameterBindingRules** para una función que devuelve un valor no null **HttpParameterBinding**.
3. En caso contrario, utilice las reglas predeterminadas que he descrito anteriormente. 

    - Si el tipo de parámetro es "sencillo" o tiene un convertidor de tipos, enlazar desde el URI. Esto equivale a poner el **[FromUri]** atributo en el parámetro.
    - En caso contrario, intenta leer el parámetro del cuerpo del mensaje. Esto equivale a poner **[FromBody]** en el parámetro.

Si deseara, podría reemplazar toda la **IActionValueBinder** servicio con una implementación personalizada.

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de enlace de parámetros personalizados](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike Stall escribió una buena serie de entradas de blog sobre el enlace de parámetro de API Web:

- [¿Cómo de API Web de enlace de parámetros](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Enlace de parámetro de estilo MVC para API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Cómo enlazar a objetos personalizados en las firmas de acción de MVC o Web API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Cómo crear un proveedor de valor personalizado en Web API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Enlace de parámetros de la API Web en segundo plano](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
