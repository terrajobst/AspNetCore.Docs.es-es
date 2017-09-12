---
title: Enlace de modelos personalizados
author: ardalis
description: Personalizar enlace de modelo en MVC de ASP.NET Core.
keywords: "Núcleo de ASP.NET, el enlace de modelos, enlazador de modelos personalizado"
ms.author: riande
manager: wpickett
ms.date: 4/10/2017
ms.topic: article
ms.assetid: ebd98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 2b95073bc0972908d0c0b2158a036e4374c7df4d
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="custom-model-binding"></a><span data-ttu-id="e29df-104">Enlace de modelos personalizados</span><span class="sxs-lookup"><span data-stu-id="e29df-104">Custom Model Binding</span></span>

<span data-ttu-id="e29df-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e29df-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e29df-106">Enlace de modelos permite acciones de controlador trabajar directamente con los tipos de modelos (pasados como argumentos de método), en su lugar que las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e29df-106">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="e29df-107">Asignación entre los modelos de datos y aplicaciones de solicitud entrantes se controla por enlazadores de modelos.</span><span class="sxs-lookup"><span data-stu-id="e29df-107">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="e29df-108">Los programadores pueden ampliar la funcionalidad de enlace de modelo integrado mediante la implementación de enlazadores de modelos personalizados (aunque por lo general, no es necesario escribir su propio proveedor).</span><span class="sxs-lookup"><span data-stu-id="e29df-108">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="e29df-109">Ver o descargar el ejemplo desde GitHub</span><span class="sxs-lookup"><span data-stu-id="e29df-109">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="e29df-110">Limitaciones del enlazador de modelo predeterminado</span><span class="sxs-lookup"><span data-stu-id="e29df-110">Default model binder limitations</span></span>

<span data-ttu-id="e29df-111">Los enlazadores de modelos predeterminado admiten la mayoría de los tipos de datos de .NET Core comunes y para cubrir necesidades de la mayoría de los desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="e29df-111">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="e29df-112">Espera que enlazar entrada basada en texto de la solicitud directamente a los tipos de modelos.</span><span class="sxs-lookup"><span data-stu-id="e29df-112">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="e29df-113">Debe transformar la entrada antes de enlazar a él.</span><span class="sxs-lookup"><span data-stu-id="e29df-113">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="e29df-114">Por ejemplo, si tiene una clave que puede usarse para buscar los datos del modelo.</span><span class="sxs-lookup"><span data-stu-id="e29df-114">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="e29df-115">Puede usar un enlazador de modelos personalizado para capturar los datos en función de la clave.</span><span class="sxs-lookup"><span data-stu-id="e29df-115">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="e29df-116">Revisión del modelo de enlace</span><span class="sxs-lookup"><span data-stu-id="e29df-116">Model binding review</span></span>

<span data-ttu-id="e29df-117">Enlace de modelo utiliza las definiciones específicas para los tipos que opera en.</span><span class="sxs-lookup"><span data-stu-id="e29df-117">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="e29df-118">A *tipo simple* se convierte en una sola cadena de la entrada.</span><span class="sxs-lookup"><span data-stu-id="e29df-118">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="e29df-119">A *tipo complejo* se convierten de varios valores de entrada.</span><span class="sxs-lookup"><span data-stu-id="e29df-119">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="e29df-120">El marco de trabajo determina la diferencia se basa en la existencia de un `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="e29df-120">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="e29df-121">Se recomienda crear un convertidor de tipos si tiene un sencillo `string`  ->  `SomeType` asignación que no necesita recursos externos.</span><span class="sxs-lookup"><span data-stu-id="e29df-121">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="e29df-122">Antes de crear su propio enlazador de modelos personalizado, es que vale la pena revisado modelo existente cómo se implementan los enlazadores.</span><span class="sxs-lookup"><span data-stu-id="e29df-122">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="e29df-123">Tenga en cuenta el [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) que puede usarse para convertir cadenas codificadas en base64 en matrices de bytes.</span><span class="sxs-lookup"><span data-stu-id="e29df-123">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="e29df-124">Las matrices de bytes se suelen almacenar como archivos o campos BLOB de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e29df-124">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="e29df-125">Trabajar con el ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="e29df-125">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="e29df-126">Cadenas codificadas en Base64 se pueden utilizar para representar datos binarios.</span><span class="sxs-lookup"><span data-stu-id="e29df-126">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="e29df-127">Por ejemplo, la siguiente imagen puede codificarse como una cadena.</span><span class="sxs-lookup"><span data-stu-id="e29df-127">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="e29df-128">![dotnet bot](custom-model-binding/images/bot.png "bot dotnet.")</span><span class="sxs-lookup"><span data-stu-id="e29df-128">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="e29df-129">Se muestra una pequeña parte de la cadena codificada en la siguiente imagen:</span><span class="sxs-lookup"><span data-stu-id="e29df-129">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="e29df-130">![bot dotnet codificado](custom-model-binding/images/encoded-bot.png "bot dotnet codificado")</span><span class="sxs-lookup"><span data-stu-id="e29df-130">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="e29df-131">Siga las instrucciones de la [archivo Léame del ejemplo](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) para convertir la cadena codificada en base64 en un archivo.</span><span class="sxs-lookup"><span data-stu-id="e29df-131">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="e29df-132">Núcleo ASP.NET MVC puede tomar un cadenas codificadas en base64 y usar un `ByteArrayModelBinder` para convertirla en una matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="e29df-132">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="e29df-133">El [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) que implementa [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) asigna `byte[]` argumentos `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="e29df-133">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="e29df-134">Al crear su propio enlazador de modelos personalizado, puede implementar su propio `IModelBinderProvider` escribir o utilizar el [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="e29df-134">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="e29df-135">En el ejemplo siguiente se muestra cómo usar `ByteArrayModelBinder` para convertir una cadena codificada en base64 en un `byte[]` y guardar el resultado en un archivo:</span><span class="sxs-lookup"><span data-stu-id="e29df-135">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

<span data-ttu-id="e29df-136">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="e29df-136">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]</span></span>

<span data-ttu-id="e29df-137">Puede publicar una cadena con codificación base64 a este método de api con una herramienta como [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="e29df-137">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="e29df-138">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="e29df-138">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="e29df-139">Siempre que el enlazador puede enlazar datos de la solicitud para las propiedades con el nombre adecuado o argumentos, enlace de modelos se realizará correctamente.</span><span class="sxs-lookup"><span data-stu-id="e29df-139">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="e29df-140">En el ejemplo siguiente se muestra cómo usar `ByteArrayModelBinder` con un modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="e29df-140">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

<span data-ttu-id="e29df-141">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="e29df-141">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]</span></span>

## <a name="custom-model-binder-sample"></a><span data-ttu-id="e29df-142">Ejemplo de enlazador de modelos personalizados</span><span class="sxs-lookup"><span data-stu-id="e29df-142">Custom model binder sample</span></span>

<span data-ttu-id="e29df-143">En esta sección se implementará un enlazador de modelos personalizados que:</span><span class="sxs-lookup"><span data-stu-id="e29df-143">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="e29df-144">Convierte los datos de solicitud entrantes en argumentos claves fuertemente tipados.</span><span class="sxs-lookup"><span data-stu-id="e29df-144">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="e29df-145">Utiliza Entity Framework Core para capturar la entidad asociada.</span><span class="sxs-lookup"><span data-stu-id="e29df-145">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="e29df-146">La entidad asociada se pasa como argumento al método de acción.</span><span class="sxs-lookup"><span data-stu-id="e29df-146">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="e29df-147">El siguiente ejemplo se utiliza la `ModelBinder` del atributo en el `Author` modelo:</span><span class="sxs-lookup"><span data-stu-id="e29df-147">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

<span data-ttu-id="e29df-148">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="e29df-148">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]</span></span>

<span data-ttu-id="e29df-149">En el código anterior, el `ModelBinder` atributo especifica el tipo de `IModelBinder` que debe utilizarse para enlazar `Author` parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="e29df-149">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="e29df-150">El `AuthorEntityBinder` se usa para enlazar un `Author` parámetro mediante la captura de la entidad de un origen de datos mediante Entity Framework Core y una `authorId`:</span><span class="sxs-lookup"><span data-stu-id="e29df-150">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

<span data-ttu-id="e29df-151">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]</span><span class="sxs-lookup"><span data-stu-id="e29df-151">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]</span></span>

<span data-ttu-id="e29df-152">El código siguiente muestra cómo utilizar el `AuthorEntityBinder` en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="e29df-152">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

<span data-ttu-id="e29df-153">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="e29df-153">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]</span></span>

<span data-ttu-id="e29df-154">El `ModelBinder` atributo se puede usar para aplicar el `AuthorEntityBinder` a los parámetros que no usan convenciones predeterminadas:</span><span class="sxs-lookup"><span data-stu-id="e29df-154">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that do not use default conventions:</span></span>

<span data-ttu-id="e29df-155">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="e29df-155">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]</span></span>

<span data-ttu-id="e29df-156">En este ejemplo, puesto que el nombre del argumento no es el valor predeterminado `authorId`, se especifica en el parámetro utilizando `ModelBinder` atributo.</span><span class="sxs-lookup"><span data-stu-id="e29df-156">In this example, since the name of the argument is not the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="e29df-157">Tenga en cuenta que el método de acción y controlador se simplifican en comparación con la búsqueda de la entidad en el método de acción.</span><span class="sxs-lookup"><span data-stu-id="e29df-157">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="e29df-158">La lógica para capturar al autor mediante Entity Framework Core se mueve al enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="e29df-158">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="e29df-159">Esto puede ser considerable simplificación cuando existen varios métodos que enlazar con el modelo de autor y pueden ayudar a seguir la [principio SECA](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="e29df-159">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="e29df-160">Puede aplicar el `ModelBinder` atributo a las propiedades de modelo individual (como en un modelo de vista) o a los parámetros de método de acción para especificar un enlazador de modelos o un nombre de modelo para simplemente ese tipo o la acción concreto.</span><span class="sxs-lookup"><span data-stu-id="e29df-160">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="e29df-161">Implementar un ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="e29df-161">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="e29df-162">En lugar de aplicar un atributo, puede implementar `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="e29df-162">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="e29df-163">Se trata cómo se implementan los enlazadores de marco de trabajo integrado.</span><span class="sxs-lookup"><span data-stu-id="e29df-163">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="e29df-164">Cuando se especifica el tipo funciona el enlazador en, especifique el tipo de argumento que se genera, **no** acepta el enlazador de la entrada.</span><span class="sxs-lookup"><span data-stu-id="e29df-164">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="e29df-165">El proveedor de enlazador siguiente funciona con la `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="e29df-165">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="e29df-166">Cuando se agrega a la colección de MVC de proveedores de, no es necesario usar el `ModelBinder` atributo `Author` o `Author` con el tipo de parámetros.</span><span class="sxs-lookup"><span data-stu-id="e29df-166">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

<span data-ttu-id="e29df-167">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]</span><span class="sxs-lookup"><span data-stu-id="e29df-167">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]</span></span>

> <span data-ttu-id="e29df-168">Nota: El código anterior devuelve un `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="e29df-168">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="e29df-169">`BinderTypeModelBinder`actúa como un generador de enlazadores de modelos y proporciona la inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="e29df-169">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="e29df-170">El `AuthorEntityBinder` requiere DI tener acceso a EF Core.</span><span class="sxs-lookup"><span data-stu-id="e29df-170">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="e29df-171">Use `BinderTypeModelBinder` si el enlazador de modelos necesita los servicios de DI.</span><span class="sxs-lookup"><span data-stu-id="e29df-171">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="e29df-172">Para utilizar un proveedor de enlazador de modelos personalizado, agréguelo en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e29df-172">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

<span data-ttu-id="e29df-173">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]</span><span class="sxs-lookup"><span data-stu-id="e29df-173">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]</span></span>

<span data-ttu-id="e29df-174">Al evaluar los enlazadores de modelos, la colección de proveedores se examina por orden.</span><span class="sxs-lookup"><span data-stu-id="e29df-174">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="e29df-175">Se utiliza el primer proveedor que devuelve un enlazador.</span><span class="sxs-lookup"><span data-stu-id="e29df-175">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="e29df-176">La siguiente imagen muestra el valor predeterminado de enlazadores de modelos desde el depurador.</span><span class="sxs-lookup"><span data-stu-id="e29df-176">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="e29df-177">![valor predeterminado de enlazadores de modelos](custom-model-binding/images/default-model-binders.png "predeterminado enlazadores de modelos")</span><span class="sxs-lookup"><span data-stu-id="e29df-177">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="e29df-178">Agregar el proveedor al final de la colección, se puede producir un enlazador de modelos integrados que se llama antes de que el enlazador predeterminado tiene una oportunidad.</span><span class="sxs-lookup"><span data-stu-id="e29df-178">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="e29df-179">En este ejemplo, el proveedor personalizado se agrega al principio de la colección para asegurarse de se utiliza para `Author` argumentos de acción.</span><span class="sxs-lookup"><span data-stu-id="e29df-179">In this example, the custom provider is added to the beginning of the collection to ensure it is used for `Author` action arguments.</span></span>

<span data-ttu-id="e29df-180">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]</span><span class="sxs-lookup"><span data-stu-id="e29df-180">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]</span></span>

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="e29df-181">Recomendaciones y prácticas recomendadas</span><span class="sxs-lookup"><span data-stu-id="e29df-181">Recommendations and best practices</span></span>

<span data-ttu-id="e29df-182">Enlazadores de modelos personalizados:</span><span class="sxs-lookup"><span data-stu-id="e29df-182">Custom model binders:</span></span>
- <span data-ttu-id="e29df-183">No debe intentar establecer códigos de estado o devolver resultados (por ejemplo, 404 no encontrado).</span><span class="sxs-lookup"><span data-stu-id="e29df-183">Should not attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="e29df-184">Si se produce un error en el enlace de modelos, un [filtro de acción](xref:mvc/controllers/filters) o lógica en el propio método de acción debe controlar los errores.</span><span class="sxs-lookup"><span data-stu-id="e29df-184">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="e29df-185">Son muy útiles para eliminar código repetitivo y problemas surgidos de corte del cruce de los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="e29df-185">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="e29df-186">Normalmente no debe usarse para convertir una cadena en un tipo personalizado, un [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) suele ser una mejor opción.</span><span class="sxs-lookup"><span data-stu-id="e29df-186">Typically should not be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
