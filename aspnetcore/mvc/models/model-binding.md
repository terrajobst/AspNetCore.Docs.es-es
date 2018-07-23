---
title: Enlace de modelos en ASP.NET Core
author: tdykstra
description: Obtenga información sobre cómo el enlace de modelos en ASP.NET Core MVC asigna datos de solicitudes HTTP a parámetros de método de acción.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 01/22/2018
uid: mvc/models/model-binding
ms.openlocfilehash: 200e2c22e02ec9e24b7cdb3883cf6f2f93f2f4b7
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095738"
---
# <a name="model-binding-in-aspnet-core"></a>Enlace de modelos en ASP.NET Core

Por [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Introducción enlace de modelos

El enlace de modelos en ASP.NET Core MVC asigna datos de solicitudes HTTP a parámetros de método de acción. Los parámetros pueden ser tipos simples, como cadenas, enteros o flotantes, o pueden ser tipos complejos. Se trata de una excelente característica de MVC porque la asignación de los datos entrantes a un equivalente es un escenario muy frecuente, independientemente del tamaño o la complejidad de los datos. MVC soluciona este problema abstrayendo el enlace para que los desarrolladores no tengan que seguir reescribiendo una versión algo diferente de ese mismo código en cada aplicación. Escribir su propio texto o código para el convertidor de tipos es una tarea aburrida y proclive a errores.

## <a name="how-model-binding-works"></a>Cómo funciona el enlace de modelos

Cuando MVC recibe una solicitud HTTP, la enruta a un método de acción específico de un controlador. Determina qué método de acción se ejecutará en función del contenido de los datos de ruta y luego enlaza valores de la solicitud HTTP a los parámetros de ese método de acción. Pongamos como ejemplo esta URL:

`http://contoso.com/movies/edit/2`

Puesto que la plantilla de ruta tiene este aspecto, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` enruta al controlador `Movies` y su método de acción `Edit`. También acepta un parámetro opcional denominado `id`. El código para el método de acción debe parecerse a esto:

```csharp
public IActionResult Edit(int? id)
   ```

Nota: Las cadenas de la ruta de URL no distinguen mayúsculas de minúsculas.

MVC intentará enlazar los datos de la solicitud a los parámetros de acción por su nombre. MVC buscará valores para cada parámetro mediante el nombre del parámetro y los nombres de sus propiedades públicas configurables. En el ejemplo anterior, el único parámetro de acción se denomina `id`, el cual MVC enlaza al valor con el mismo nombre en los valores de ruta. Además de los valores de ruta, MVC enlazará datos de varias partes de la solicitud y lo hará en un orden establecido. Esta es una lista de los orígenes de datos en el orden en que el enlace de modelos busca en ellos:

1. `Form values`: son valores de formulario que van en la solicitud HTTP mediante el método POST. (incluidas las solicitudes POST de jQuery).

2. `Route values`: conjunto de valores de ruta proporcionados por el [enrutamiento](xref:fundamentals/routing)

3. `Query strings`: elemento de cadena de consulta del URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Nota: Los valores de formulario, los datos de enrutamiento y las cadenas de consulta se almacenan como pares de nombre-valor.

Como el enlace de modelos pidió una clave con el nombre `id` y no hay nada denominado `id` en los valores de formulario, continuó con los valores de ruta buscando esa clave. En nuestro ejemplo, es una coincidencia. Se produce el enlace y el valor se convierte en el entero 2. La misma solicitud mediante Edit(string id) se convertiría en la cadena "2".

Hasta ahora en el ejemplo se usan tipos simples. En MVC, los tipos simples son cualquier tipo o tipo primitivo de .NET con un convertidor de tipos de cadena. Si el parámetro del método de acción fuera una clase como el tipo `Movie`, que contiene tipos simples y complejos como propiedades, el enlace de modelos de MVC seguiría controlándolo correctamente. Usa reflexión y recursividad para recorrer las propiedades de tipos complejos buscando coincidencias. El enlace de modelos busca el patrón *parameter_name.property_name* para enlazar los valores a las propiedades. Si no encuentra los valores coincidentes con esta forma, intentará enlazar usando solo el nombre de propiedad. Para esos tipos, como los tipos `Collection`, el enlace de modelos busca coincidencias para *parameter_name[index]* o solo *[index]*. En enlace de modelos trata los tipos `Dictionary` del mismo modo, preguntando por *parameter_name [key]* o simplemente *[key]*, siempre que las claves sean tipos simples. Las claves compatibles coinciden con el HTML de nombres de campos y las aplicaciones auxiliares de etiquetas generadas para el mismo tipo de modelo. Esto permite realizar un recorrido de ida y vuelta para que los campos del formulario permanezcan rellenados con los datos del usuario para su comodidad (por ejemplo, cuando los datos enlazados de una creación o una edición no superaron la validación).

Para que se produzca el enlace, la clase debe tener un constructor público predeterminado y el miembro que se va a enlazar debe ser una propiedad pública de escritura. Cuando se produce el enlace de modelos, la instancia de la clase se creará usando solamente el constructor predeterminado público y, después, ya se podrán establecer las propiedades.

Cuando se enlaza un parámetro, el enlace de modelos deja de buscar valores con ese nombre y pasa a enlazar el siguiente parámetro. En caso contrario, el comportamiento predeterminado del enlace de modelos establece los parámetros en sus valores predeterminados según el tipo:

* `T[]`: con la excepción de matrices de tipo `byte[]`, el enlace establece parámetros de tipo `T[]` a `Array.Empty<T>()`. Las matrices de tipo `byte[]` se establecen en `null`.

* Tipos de referencia: el enlace crea una instancia de una clase con el constructor predeterminado sin tener que configurar propiedades. Pero el enlace de modelos establece parámetros `string` en `null`.

* Tipos que aceptan valores NULL: estos tipos se establecen en `null`. En el ejemplo anterior, el enlace de modelos establece `id` en `null` ya que es de tipo `int?`.

* Tipos de valor: los tipos de valor que no aceptan valores NULL de tipo `T` se establecen en `default(T)`. Por ejemplo, el enlace de modelos establecerá un parámetro `int id` en 0. Considere la posibilidad de usar la validación de modelos o los tipos que aceptan valores NULL en lugar de depender de valores predeterminados.

Si se produce un error de enlace, MVC no genera un error. Todas las acciones que aceptan datos del usuario deben comprobar la propiedad `ModelState.IsValid`.

Nota: Cada entrada en la propiedad `ModelState` del controlador es un elemento `ModelStateEntry` que contiene una propiedad `Errors`. No suele ser necesario consultar esta colección por su cuenta. Utilice `ModelState.IsValid` en su lugar.

Además, hay algunos tipos de datos especiales que MVC debe tener en cuenta al realizar el enlace de modelos:

* `IFormFile`, `IEnumerable<IFormFile>`: uno o más archivos cargados que forman parte de la solicitud HTTP.

* `CancellationToken`: se usa para cancelar la actividad en controladores asincrónicos.

Estos tipos se pueden enlazar a parámetros de acción o a propiedades en un tipo de clase.

Cuando se completa el enlace de modelos, se produce la [validación](validation.md). El enlace de modelos predeterminado funciona muy bien para la amplia mayoría de escenarios de desarrollo. También es extensible, por lo que si tiene necesidades únicas, puede personalizar el comportamiento integrado.

## <a name="customize-model-binding-behavior-with-attributes"></a>Personalizar el comportamiento de enlace de modelos con atributos

MVC contiene varios atributos que puede usar para dirigir su comportamiento de enlace de modelos predeterminado a un origen diferente. Por ejemplo, puede especificar si se requiere el enlace para una propiedad o si nunca debe ocurrir en absoluto mediante el uso de los atributos `[BindRequired]` o `[BindNever]`. Como alternativa, puede reemplazar el origen de datos predeterminado y especificar el origen de datos del enlazador de modelos. En esta lista se muestran los atributos del enlace de modelos:

* `[BindRequired]`: este atributo agrega un error al estado de modelo si no se puede producir el enlace.

* `[BindNever]`: indica al enlazador de modelos que nunca enlace a este parámetro.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: use estos para especificar el origen de enlace exacto que quiere aplicar.

* `[FromServices]`: este atributo usa la [inserción de dependencias](../../fundamentals/dependency-injection.md) para enlazar parámetros de los servicios.

* `[FromBody]`: use los formateadores configurados para enlazar datos desde el cuerpo de solicitud. El formateador se selecciona según el tipo de contenido de la solicitud.

* `[ModelBinder]`: se usa para reemplazar el enlazador de modelos predeterminado, el origen del enlace y el nombre.

Los atributos son herramientas muy útiles cuando es necesario invalidar el comportamiento predeterminado del enlace de modelos.

## <a name="bind-formatted-data-from-the-request-body"></a>Enlazar datos con formato desde el cuerpo de la solicitud

Los datos de la solicitud pueden proceder de una variedad de formatos como JSON, XML y muchos otros. Cuando se usa el atributo [FromBody] para indicar que quiere enlazar un parámetro a los datos en el cuerpo de la solicitud, MVC usa un conjunto de formateadores configurado para administrar los datos de la solicitud en función de su tipo de contenido. De forma predeterminada, MVC incluye una clase `JsonInputFormatter` para controlar datos JSON, pero puede agregar formateadores adicionales para administrar XML y otros formatos personalizados.

> [!NOTE]
> Puede haber como máximo un parámetro por cada acción decorada con `[FromBody]`. El tiempo de ejecución de ASP.NET Core MVC delega la responsabilidad de leer la secuencia de solicitudes al formateador. Una vez que se lee la secuencia de solicitudes para un parámetro, por lo general no es posible leer la secuencia de solicitudes de nuevo para enlazar otros parámetros `[FromBody]`.

> [!NOTE]
> `JsonInputFormatter` es el formateador predeterminado y se basa en [Json.NET](https://www.newtonsoft.com/json).

ASP.NET selecciona formateadores de entradas basándose en el encabezado [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) y el tipo del parámetro, a menos que haya un atributo aplicado a él que especifique lo contrario. Si quiere usar XML u otro formato, debe configurarlo en el archivo *Startup.cs*, pero primero tiene que obtener una referencia a `Microsoft.AspNetCore.Mvc.Formatters.Xml` mediante NuGet. El código de inicio debe tener este aspecto:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

El código del archivo *Startup.cs* contiene un método `ConfigureServices` con un argumento `services` que puede usar para crear servicios para la aplicación ASP.NET. En el ejemplo, vamos a agregar un formateador XML como un servicio que MVC proporcionará para esta aplicación. El argumento `options` pasado al método `AddMvc` permite agregar y administrar filtros, formateadores y otras opciones del sistema desde MVC al inicio de la aplicación. Después, aplique el atributo `Consumes` a las clases de controlador o a los métodos de acción para que funcione con el formato que quiera.

### <a name="custom-model-binding"></a>Enlace de modelos personalizado

Puede ampliar el enlace de modelos escribiendo sus propios enlazadores de modelos personalizados. Más información sobre el [enlace de modelos personalizados](../advanced/custom-model-binding.md).
