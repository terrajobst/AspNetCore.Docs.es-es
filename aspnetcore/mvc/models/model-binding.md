---
title: Enlace de modelos
author: rachelappel
description: "Información sobre el enlace de modelo en MVC de ASP.NET Core"
ms.author: rachelap
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
uid: mvc/models/model-binding
ms.openlocfilehash: 8fc6ff66d05164c1040f8cc77886357a633a0472
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2018
---
# <a name="model-binding"></a>Enlace de modelos

Por [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Introducción al modelo de enlace

Enlace de modelo en MVC de ASP.NET Core asigna datos de las solicitudes HTTP a los parámetros de método de acción. Los parámetros pueden ser tipos simples como cadenas, enteros o flotantes, o pueden ser tipos complejos. Se trata de una característica excelente de MVC porque la asignación de los datos entrantes a un equivalente es un escenario a menudo repetido, independientemente del tamaño o complejidad de los datos. MVC soluciona este problema abstrayendo enlace para que los desarrolladores no tienen que mantener volver a escribir una versión ligeramente diferente de ese mismo código en cada aplicación. Escribir su propio texto para escribir el código del convertidor es tedioso y es proclive a errores.

## <a name="how-model-binding-works"></a>Cómo funciona el enlace de modelo

Cuando MVC recibe una solicitud HTTP, enruta a un método de acción específica de un controlador. Determina qué método de acción para ejecutar en función de lo que está en los datos de ruta, a continuación, enlaza los valores de la solicitud HTTP a parámetros de ese método de acción. Por ejemplo, considere la siguiente dirección URL:

`http://contoso.com/movies/edit/2`

Puesto que la plantilla de ruta tiene un aspecto parecido a éste, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` enruta a la `Movies` controlador y su `Edit` método de acción. También acepta un parámetro opcional denominado `id`. El código para el método de acción debe tener un aspecto similar al siguiente:

```csharp
public IActionResult Edit(int? id)
   ```

Nota: Las cadenas de la ruta de dirección URL no distinguen mayúsculas de minúsculas.

MVC intentará enlazar los datos de la solicitud a los parámetros de acción por su nombre. MVC buscará valores para cada parámetro con el nombre del parámetro y los nombres de sus propiedades públicas configurables. En el ejemplo anterior, el parámetro de acción solo se denomina `id`, que MVC enlaza con el valor con el mismo nombre en los valores de ruta. Además de los valores de ruta MVC enlazará los datos de varias partes de la solicitud y lo hace en un orden establecido. A continuación se muestra una lista de los orígenes de datos en el orden en que se busca el enlace de modelos a través de ellos:

1. `Form values`: Estos son los valores de formulario que van en la solicitud HTTP que utilizan el método POST. (incluidas las solicitudes POST de jQuery).

2. `Route values`: El conjunto de valores de la ruta proporcionada por [enrutamiento](xref:fundamentals/routing)

3. `Query strings`: La parte de la cadena de consulta del URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Nota: Formulario valores, enrutar los datos y cadenas se almacenan como pares de nombre y valor de consulta.

Puesto que el enlace de modelos pedirá una clave denominada `id` y no hay nada denominado `id` en los valores del formulario, mueven en para los valores de ruta busca esa clave. En nuestro ejemplo, es una coincidencia. Se produce el enlace y el valor se convierte en un entero de 2. La misma solicitud mediante la edición (Id. de la cadena) se convertiría en la cadena "2".

Hasta ahora en el ejemplo se utiliza tipos simples. En MVC tipos simples son cualquier tipo primitivo de .NET o con un convertidor de tipos de cadena. Si el parámetro del método de acción fuera una clase como el `Movie` tipo, que contiene tipos simples y complejos, como propiedades, modelo enlace seguirán del MVC controlen correctamente. Usa reflexión y recursividad para recorrer las propiedades de tipos complejos buscando coincidencias. Enlace de modelos busca el patrón *parameter_name.property_name* para enlazar los valores a las propiedades. Si no encuentra los valores correspondientes de esta forma, intentará enlazar con el nombre de propiedad. Para esos tipos como `Collection` tipos, el enlace de modelos busca las coincidencias a *parameter_name [índice]* o simplemente *[índice]*. Trata de enlace de modelo `Dictionary` del mismo modo, los tipos piden *parameter_name [key]* o simplemente *[key]*, siempre que las claves son tipos simples. Las claves que se admiten coincide con los nombres de campo HTML y aplicaciones auxiliares de etiquetas generadas para el mismo tipo de modelo. Esto permite que los valores de ida y vuelta para que los campos de formulario se mantienen repleto de la entrada del usuario para su comodidad, por ejemplo, cuando los datos enlazados de una creación o edición no superan la validación.

En el orden de enlace que se produzca la clase debe tener un constructor público predeterminado y miembro esté enlazado debe ser propiedades públicas de escritura. Cuando se produzca el enlazado de modelo que solo se crearán instancias de la clase utilizando el constructor predeterminado público, pueden establecer las propiedades.

Cuando se enlaza un parámetro, el enlace de modelos detiene buscando valores con ese nombre y lo pasa a enlazar el parámetro siguiente. En caso contrario, el comportamiento de enlace de modelo predeterminado establece los parámetros con sus valores predeterminados según su tipo:

* `T[]`: Con la excepción de las matrices de tipo `byte[]`, enlace establece los parámetros de tipo `T[]` a `Array.Empty<T>()`. Matrices de tipo `byte[]` se establecen en `null`.

* Tipos de referencia: Enlace crea una instancia de una clase con el constructor predeterminado sin tener que configurar propiedades. Sin embargo, los conjuntos de enlace del modelo `string` parámetros para `null`.

* Tipos que aceptan valores NULL: Los tipos que aceptan valores NULL se establecen en `null`. En el ejemplo anterior, modelo de enlace configura `id` a `null` ya que es de tipo `int?`.

* Tipos de valor: Tipos de valor no acepta valores NULL del tipo `T` se establecen en `default(T)`. Por ejemplo, el enlace de modelos para establecer un parámetro `int id` en 0. Considere la posibilidad de usa validación de modelos o tipos que aceptan valores NULL en lugar de basarse en valores predeterminados.

Si se produce un error de enlace, MVC no produce un error. Todas las acciones que acepta proporcionados por el usuario deben comprobar la `ModelState.IsValid` propiedad.

Nota: Cada entrada en el controlador `ModelState` propiedad es una `ModelStateEntry` que contiene un `Errors` propiedad. Es no suele ser necesario consultar esta colección usted mismo. Utilice `ModelState.IsValid` en su lugar.

Además, hay algunos tipos de datos especiales que MVC debe tener en cuenta al realizar el enlace de modelos:

* `IFormFile`, `IEnumerable<IFormFile>`: Uno o más archivos cargados que forman parte de la solicitud HTTP.

* `CancellationToken`: Se utiliza para cancelar la actividad en controladores asincrónicos.

Estos tipos se pueden enlazar a los parámetros de acción o a las propiedades en un tipo de clase.

Cuando se complete, el enlace de modelos [validación](validation.md) se produce. Enlace de modelo predeterminado funciona muy bien para la mayoría de escenarios de desarrollo. También es extensible para que si tiene necesidades únicas que pueda personalizar el comportamiento integrado.

## <a name="customize-model-binding-behavior-with-attributes"></a>Personalizar el comportamiento de enlace de modelo con atributos

MVC contiene varios atributos que puede usar para dirigir su comportamiento de enlace de modelo predeterminado para un origen diferente. Por ejemplo, puede especificar si se requiere para una propiedad de enlace, o si nunca debe ocurrir en absoluto mediante el uso de la `[BindRequired]` o `[BindNever]` atributos. Como alternativa, puede reemplazar el origen de datos predeterminado y especificar el origen de datos del enlazador de modelos. A continuación se muestra una lista de atributos de enlace de modelo:

* `[BindRequired]`: Este atributo agrega un error de estado de modelo si no se puede producir el enlace.

* `[BindNever]`: Indica el enlazador de modelos para nunca se enlazan con este parámetro.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Utilícelos para especificar el origen de enlace exacto que desea aplicar.

* `[FromServices]`: Este atributo usa [inyección de dependencia](../../fundamentals/dependency-injection.md) para enlazar parámetros de servicios.

* `[FromBody]`: Use los formateadores configurados para enlazar datos desde el cuerpo de solicitud. El formateador se selecciona basándose en el tipo de contenido de la solicitud.

* `[ModelBinder]`: Se utiliza para reemplazar el enlazador de modelos predeterminado, el origen de enlace y el nombre.

Atributos son herramientas muy útiles cuando es necesario invalidar el comportamiento predeterminado del enlace del modelo.

## <a name="bind-formatted-data-from-the-request-body"></a>Enlazar datos con formato desde el cuerpo de solicitud

Datos de la solicitud pueden proceder de una variedad de formatos como JSON, XML y muchos otros. Cuando se utiliza el atributo [FromBody] para indicar que desea enlazar un parámetro a los datos en el cuerpo de solicitud, MVC usa un conjunto de formateadores configurado para controlar los datos de solicitud en función de su tipo de contenido. De forma predeterminada, MVC incluye un `JsonInputFormatter` la clase para controlar datos JSON, pero puede agregar formateadores adicionales para el tratamiento de XML y otros formatos personalizados.

> [!NOTE]
> Puede haber a lo sumo un parámetro por cada acción decorada con `[FromBody]`. El tiempo de ejecución de MVC de ASP.NET Core delega la responsabilidad de leer la secuencia de solicitud en el formateador. Una vez que se lee la secuencia de solicitud para un parámetro, por lo general no es posible leer la secuencia de solicitud nuevo para otro enlace `[FromBody]` parámetros.

> [!NOTE]
> El `JsonInputFormatter` es el formateador predeterminado y se basa en [Json.NET](https://www.newtonsoft.com/json).

ASP.NET selecciona entradas formateadores tomando como base la [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) encabezado y el tipo del parámetro, a menos que haya un atributo aplicado a especificando en caso contrario. Si le gustaría usar XML u otro formato, debe configurarla en el *Startup.cs* archivo, pero primero tiene que obtener una referencia a `Microsoft.AspNetCore.Mvc.Formatters.Xml` mediante NuGet. El código de inicio debe tener un aspecto similar al siguiente:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

El código de la *Startup.cs* archivo contiene un `ConfigureServices` método con un `services` argumento puede utilizar para generar los servicios para la aplicación ASP.NET. En el ejemplo, vamos a agregar a un formateador XML como un servicio que MVC proporcione para esta aplicación. El `options` argumento pasado a la `AddMvc` método le permite agregar y administrar filtros, los formateadores y otras opciones de sistema de MVC al inicio de la aplicación. A continuación, aplique la `Consumes` atributo a las clases de controlador o los métodos de acción para que funcione con el formato que desee.

### <a name="custom-model-binding"></a>Enlace de modelos personalizados

Puede ampliar el enlace de modelos escribiendo sus propio enlazadores de modelos personalizados. Obtenga más información sobre [enlace de modelos personalizados](../advanced/custom-model-binding.md).
