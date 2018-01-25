---
title: Enlace de modelos personalizados
author: ardalis
description: Personalizar enlace de modelo en MVC de ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 85d5ca18944e774d1f2577459c6c45acde01e4d9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="custom-model-binding"></a>Enlace de modelos personalizados

Por [Steve Smith](https://ardalis.com/)

Enlace de modelos permite acciones de controlador trabajar directamente con los tipos de modelos (pasados como argumentos de método), en su lugar que las solicitudes HTTP. Asignación entre los modelos de datos y aplicaciones de solicitud entrantes se controla por enlazadores de modelos. Los programadores pueden ampliar la funcionalidad de enlace de modelo integrado mediante la implementación de enlazadores de modelos personalizados (aunque por lo general, no es necesario escribir su propio proveedor).

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Limitaciones del enlazador de modelo predeterminado

Los enlazadores de modelos predeterminado admiten la mayoría de los tipos de datos de .NET Core comunes y para cubrir necesidades de la mayoría de los desarrolladores. Espera que enlazar entrada basada en texto de la solicitud directamente a los tipos de modelos. Debe transformar la entrada antes de enlazar a él. Por ejemplo, si tiene una clave que puede usarse para buscar los datos del modelo. Puede usar un enlazador de modelos personalizado para capturar los datos en función de la clave.

## <a name="model-binding-review"></a>Revisión del modelo de enlace

Enlace de modelo utiliza las definiciones específicas para los tipos que opera en. A *tipo simple* se convierte en una sola cadena de la entrada. A *tipo complejo* se convierten de varios valores de entrada. El marco de trabajo determina la diferencia se basa en la existencia de un `TypeConverter`. Se recomienda crear un convertidor de tipos si tiene un sencillo `string`  ->  `SomeType` asignación que no necesita recursos externos.

Antes de crear su propio enlazador de modelos personalizado, es que vale la pena revisado modelo existente cómo se implementan los enlazadores. Tenga en cuenta el [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) que puede usarse para convertir cadenas codificadas en base64 en matrices de bytes. Las matrices de bytes se suelen almacenar como archivos o campos BLOB de base de datos.

### <a name="working-with-the-bytearraymodelbinder"></a>Trabajar con el ByteArrayModelBinder

Cadenas codificadas en Base64 se pueden utilizar para representar datos binarios. Por ejemplo, la siguiente imagen puede codificarse como una cadena.

![dotnet bot](custom-model-binding/images/bot.png "bot dotnet.")

Se muestra una pequeña parte de la cadena codificada en la siguiente imagen:

![bot dotnet codificado](custom-model-binding/images/encoded-bot.png "bot dotnet codificado")

Siga las instrucciones de la [archivo Léame del ejemplo](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) para convertir la cadena codificada en base64 en un archivo.

Núcleo ASP.NET MVC puede tomar un cadenas codificadas en base64 y usar un `ByteArrayModelBinder` para convertirla en una matriz de bytes. El [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) que implementa [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) asigna `byte[]` argumentos `ByteArrayModelBinder`:

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

Al crear su propio enlazador de modelos personalizado, puede implementar su propio `IModelBinderProvider` escribir o utilizar el [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).

En el ejemplo siguiente se muestra cómo usar `ByteArrayModelBinder` para convertir una cadena codificada en base64 en un `byte[]` y guardar el resultado en un archivo:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Puede publicar una cadena con codificación base64 a este método de api con una herramienta como [Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Siempre que el enlazador puede enlazar datos de la solicitud para las propiedades con el nombre adecuado o argumentos, enlace de modelos se realizará correctamente. En el ejemplo siguiente se muestra cómo usar `ByteArrayModelBinder` con un modelo de vista:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Ejemplo de enlazador de modelos personalizados

En esta sección se implementará un enlazador de modelos personalizados que:

- Convierte los datos de solicitud entrantes en argumentos claves fuertemente tipados.
- Utiliza Entity Framework Core para capturar la entidad asociada.
- La entidad asociada se pasa como argumento al método de acción.

El siguiente ejemplo se utiliza la `ModelBinder` del atributo en el `Author` modelo:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

En el código anterior, el `ModelBinder` atributo especifica el tipo de `IModelBinder` que debe utilizarse para enlazar `Author` parámetros de acción. 

El `AuthorEntityBinder` se usa para enlazar un `Author` parámetro mediante la captura de la entidad de un origen de datos mediante Entity Framework Core y una `authorId`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

El código siguiente muestra cómo utilizar el `AuthorEntityBinder` en un método de acción:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

El `ModelBinder` atributo se puede usar para aplicar el `AuthorEntityBinder` a los parámetros que no usan convenciones predeterminadas:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

En este ejemplo, puesto que el nombre del argumento no es el valor predeterminado `authorId`, se especifica en el parámetro utilizando `ModelBinder` atributo. Tenga en cuenta que el método de acción y controlador se simplifican en comparación con la búsqueda de la entidad en el método de acción. La lógica para capturar al autor mediante Entity Framework Core se mueve al enlazador de modelos. Esto puede ser considerable simplificación cuando existen varios métodos que enlazar con el modelo de autor y pueden ayudar a seguir la [principio SECA](http://deviq.com/don-t-repeat-yourself/).

Puede aplicar el `ModelBinder` atributo a las propiedades de modelo individual (como en un modelo de vista) o a los parámetros de método de acción para especificar un enlazador de modelos o un nombre de modelo para simplemente ese tipo o la acción concreto.

### <a name="implementing-a-modelbinderprovider"></a>Implementar un ModelBinderProvider

En lugar de aplicar un atributo, puede implementar `IModelBinderProvider`. Se trata cómo se implementan los enlazadores de marco de trabajo integrado. Cuando se especifica el tipo funciona el enlazador en, especifique el tipo de argumento que se genera, **no** acepta el enlazador de la entrada. El proveedor de enlazador siguiente funciona con la `AuthorEntityBinder`. Cuando se agrega a la colección de MVC de proveedores de, no es necesario usar el `ModelBinder` atributo `Author` o `Author` con el tipo de parámetros.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Nota: El código anterior devuelve un `BinderTypeModelBinder`. `BinderTypeModelBinder`actúa como un generador de enlazadores de modelos y proporciona la inserción de dependencias (DI). El `AuthorEntityBinder` requiere DI tener acceso a EF Core. Use `BinderTypeModelBinder` si el enlazador de modelos necesita los servicios de DI.

Para utilizar un proveedor de enlazador de modelos personalizado, agréguelo en `ConfigureServices`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Al evaluar los enlazadores de modelos, la colección de proveedores se examina por orden. Se utiliza el primer proveedor que devuelve un enlazador.

La siguiente imagen muestra el valor predeterminado de enlazadores de modelos desde el depurador.

![valor predeterminado de enlazadores de modelos](custom-model-binding/images/default-model-binders.png "predeterminado enlazadores de modelos")

Agregar el proveedor al final de la colección, se puede producir un enlazador de modelos integrados que se llama antes de que el enlazador predeterminado tiene una oportunidad. En este ejemplo, el proveedor personalizado se agrega al principio de la colección para asegurarse de se utiliza para `Author` argumentos de acción.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Recomendaciones y prácticas recomendadas

Enlazadores de modelos personalizados:
- No debe intentar establecer códigos de estado o devolver resultados (por ejemplo, 404 no encontrado). Si se produce un error en el enlace de modelos, un [filtro de acción](xref:mvc/controllers/filters) o lógica en el propio método de acción debe controlar los errores.
- Son muy útiles para eliminar código repetitivo y problemas surgidos de corte del cruce de los métodos de acción.
- Por lo general no deben usarse para convertir una cadena en un tipo personalizado, un [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) suele ser una mejor opción.
