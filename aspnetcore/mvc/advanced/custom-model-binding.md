---
title: Enlace de modelos personalizado en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo el enlace de modelos permite que las acciones de controlador funcionen directamente con tipos de modelos en ASP.NET Core.
ms.author: riande
ms.date: 11/13/2018
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 91f42393ffee3249f9167e10eaea7b279a7cb70b
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878413"
---
# <a name="custom-model-binding-in-aspnet-core"></a>Enlace de modelos personalizado en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Con el enlace de modelos, las acciones de controlador pueden funcionar directamente con tipos de modelos (pasados como argumentos de método), en lugar de con solicitudes HTTP. La asignación entre los datos de solicitudes entrantes y los modelos de aplicaciones se controla por medio de enlazadores de modelos. Los desarrolladores pueden ampliar la funcionalidad integrada de enlace de modelos implementando enlazadores de modelos personalizados (si bien, por lo general, no es necesario escribir un proveedor propio).

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Limitaciones de los enlazadores de modelos predeterminados

Los enlazadores de modelos predeterminados admiten la mayoría de los tipos de datos de .NET Core comunes y deberían cubrir las necesidades de casi cualquier desarrollador. Esperan enlazar entradas basadas en texto desde la solicitud directamente a tipos de modelos. Puede que sea necesario transformar la entrada antes de enlazarla. Es el caso, por ejemplo, si tiene una clave que se puede usar para buscar datos del modelo. Se puede usar un enlazador de modelos personalizado para capturar datos en función de la clave.

## <a name="model-binding-review"></a>Revisión del enlace de modelos

El enlace de modelos usa definiciones específicas de los tipos con los que funciona. Un *tipo simple* se convierte a partir de una sola cadena de la entrada, mientras que un *tipo complejo* se convierte a partir de varios valores de entrada. El marco establece la diferencia dependiendo de si existe un `TypeConverter`. Conviene crear un convertidor de tipos si existe una asignación simple `string` -> `SomeType` que no necesita recursos externos.

Antes de crear su propio enlazador de modelos personalizado, no está de más que repase cómo se implementan los enlazadores de modelos existentes. Hay que considerar el uso de [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder), que sirve para convertir cadenas codificadas con base64 en matrices de bytes. Las matrices de bytes se suelen almacenar como archivos o como campos de tipo BLOB de base de datos.

### <a name="working-with-the-bytearraymodelbinder"></a>Trabajar con ByteArrayModelBinder

Las cadenas codificadas con base64 se pueden usar para representar datos binarios. Por ejemplo, la siguiente imagen se puede codificar como una cadena.

![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")

En la siguiente imagen se muestra una pequeña porción de la cadena codificada:

![dotnet bot codificada](custom-model-binding/images/encoded-bot.png "dotnet bot codificada")

Siga las instrucciones del [archivo Léame del ejemplo](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) para convertir la cadena codificada con base64 en un archivo.

ASP.NET Core MVC toma cadenas codificadas con base64 y usa un `ByteArrayModelBinder` para convertirlas en una matriz de bytes. [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider), que implementa [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider), asigna argumentos `byte[]` a `ByteArrayModelBinder`:

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

Cuando cree su propio enlazador de modelos personalizado, puede implementar su tipo `IModelBinderProvider` particular o usar [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).

En el siguiente ejemplo se indica cómo usar `ByteArrayModelBinder` para convertir una cadena codificada con base64 en un `byte[]` y guardar el resultado en un archivo:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Se puede usar un método API POST en una cadena codificada con base64 con una herramienta como [Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Siempre y cuando el enlazador pueda enlazar datos de la solicitud a argumentos o propiedades con el nombre adecuado, el enlace de modelos se realizará correctamente. En el siguiente ejemplo se muestra cómo usar `ByteArrayModelBinder` con un modelo de vista:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Ejemplo de enlazador de modelos personalizado

En esta sección implementaremos un enlazador de modelos personalizado que haga lo siguiente:

- Convertir los datos de solicitud entrantes en argumentos clave fuertemente tipados
- Usar Entity Framework Core para capturar la entidad asociada
- Pasar la entidad asociada como argumento al método de acción

En el siguiente ejemplo se usa el atributo `ModelBinder` en el modelo `Author`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

En el código anterior, el atributo `ModelBinder` especifica el tipo de `IModelBinder` que se debe emplear para enlazar parámetros de acción de `Author`.

La clase `AuthorEntityBinder` siguiente enlaza un parámetro `Author` capturando la entidad de un origen de datos por medio de Entity Framework Core y un elemento `authorId`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> La clase `AuthorEntityBinder` anterior está diseñada para ilustrar un enlazador de modelos personalizado. La clase no está pensada para ilustrar los procedimientos recomendados para un escenario de búsqueda. Para la búsqueda, enlace el valor `authorId` y consulte la base de datos en un método de acción. Este enfoque permite diferenciar y separar los errores de enlace de modelo de los casos `NotFound`.

En el siguiente código se indica cómo usar `AuthorEntityBinder` en un método de acción:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

El atributo `ModelBinder` se puede usar para aplicar `AuthorEntityBinder` a los parámetros que no usan convenciones predeterminadas:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

En este ejemplo, como el nombre del argumento no es el `authorId` predeterminado, se especifica en el parámetro por medio del atributo `ModelBinder`. Tanto el controlador como el método de acción se simplifican, en contraste con tener que buscar la entidad en el método de acción. La lógica para capturar el autor a través de Entity Framework Core se traslada al enlazador de modelos. Esto puede reducir enormemente la complejidad cuando existen varios métodos que se enlazan al modelo `Author`.

Puede aplicar el atributo `ModelBinder` a propiedades de modelo individuales (como en un modelo de vista) o a parámetros del método de acción para especificar un determinado nombre de modelo o enlazador de modelos que sea exclusivo de ese tipo o acción en particular.

### <a name="implementing-a-modelbinderprovider"></a>Implementar un ModelBinderProvider

En lugar de aplicar un atributo, puede implementar `IModelBinderProvider`. Así es como se implementan los enlazadores de marco integrados. Cuando se especifica el tipo con el que el enlazador funciona, lo que se está indicando es el tipo de argumento que ese enlazador genera, **no** la entrada que acepta. El siguiente proveedor de enlazador funciona con `AuthorEntityBinder`. Cuando se agrega a la colección de proveedores de MVC, no es necesario usar el atributo `ModelBinder` en los parámetros con tipo `Author` o `Author`.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Nota: El código anterior devuelve un `BinderTypeModelBinder`. `BinderTypeModelBinder` actúa como una fábrica de enlazadores de modelos y proporciona la inserción de dependencias. `AuthorEntityBinder` requiere que la inserción de dependencias tenga acceso a Entity Framework Core. Use `BinderTypeModelBinder` si su enlazador de modelos necesita servicios de inserción de dependencias.

Para usar un proveedor de enlazadores de modelos personalizado, agréguelo a `ConfigureServices`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Al evaluar enlazadores de modelos, la colección de proveedores se examina en orden. Se usará el primer proveedor que devuelva un enlazador.

En la siguiente imagen se muestran los enlazadores de modelos predeterminados en el depurador.

![enlazadores de modelos predeterminados](custom-model-binding/images/default-model-binders.png "enlazadores de modelos predeterminados")

Si su proveedor se agrega al final de la colección, puede ocurrir que se llame a un enlazador de modelos integrado antes que al suyo. En este ejemplo, el proveedor personalizado se agrega al principio de la colección para procurar que se use en los argumentos de acción de `Author`.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

### <a name="polymorphic-model-binding"></a>Enlace de modelos polimórfico

El enlace a diferentes modelos de tipos derivados se conoce como enlace de modelos polimórfico. El enlace de modelos personalizado es necesario cuando el valor de la solicitud se debe enlazar al tipo de modelo derivado específico. A menos que se requiera este enfoque, se recomienda evitar el enlace de modelos polimórfico. El enlace de modelos polimórfico dificulta el razonamiento acerca de los modelos enlazados. Sin embargo, si una aplicación requiere el enlace de modelos polimórfico, una implementación podría ser similar al código siguiente:

El enlace a diferentes modelos de tipos derivados se conoce como enlace de modelos polimórfico. El enlace de modelos personalizado es necesario cuando el valor de la solicitud se debe enlazar al tipo de modelo derivado específico. Enlace de modelos polimórfico:

* No es habitual para una API REST diseñada para interoperar con todos los lenguajes.
* Dificulta el razonamiento sobre los modelos enlazados.

Sin embargo, si una aplicación requiere el enlace de modelos polimórfico, una implementación podría ser similar al código siguiente:

[!code-csharp[](custom-model-binding/3.0sample/PolymorphicModelBinding/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a>Sugerencias y procedimientos recomendados

Los enlazadores de modelos personalizados deben caracterizarse por lo siguiente:

- No deben tratar de establecer códigos de estado ni devolver resultados (por ejemplo, 404 No encontrado). Los errores que se produzcan en un enlace de modelos se deben controlar con un [filtro de acciones](xref:mvc/controllers/filters) o con la lógica del propio método de acción.
- Son realmente útiles para eliminar el código repetitivo y las cuestiones transversales de los métodos de acción.
- No se deben usar en general para convertir una cadena en un tipo personalizado. Para ello, [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) suele ser una mejor opción.
