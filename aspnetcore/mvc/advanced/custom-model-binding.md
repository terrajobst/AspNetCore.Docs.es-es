---
title: Enlace de modelos personalizado
author: ardalis
description: "Personalización del enlace de modelos en ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 04/10/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 313bc586a1c313f0bf5d8f413a4b082ffc2b7f0c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="custom-model-binding"></a><span data-ttu-id="b1354-103">Enlace de modelos personalizado</span><span class="sxs-lookup"><span data-stu-id="b1354-103">Custom Model Binding</span></span>

<span data-ttu-id="b1354-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b1354-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b1354-105">Con el enlace de modelos, las acciones de controlador pueden funcionar directamente con tipos de modelos (pasados como argumentos de método), en lugar de con solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b1354-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="b1354-106">La asignación entre los datos de solicitudes entrantes y los modelos de aplicaciones se controla por medio de enlazadores de modelos.</span><span class="sxs-lookup"><span data-stu-id="b1354-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="b1354-107">Los desarrolladores pueden ampliar la funcionalidad integrada de enlace de modelos implementando enlazadores de modelos personalizados (si bien, por lo general, no es necesario escribir un proveedor propio).</span><span class="sxs-lookup"><span data-stu-id="b1354-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="b1354-108">Ver o descargar el ejemplo desde GitHub</span><span class="sxs-lookup"><span data-stu-id="b1354-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="b1354-109">Limitaciones de los enlazadores de modelos predeterminados</span><span class="sxs-lookup"><span data-stu-id="b1354-109">Default model binder limitations</span></span>

<span data-ttu-id="b1354-110">Los enlazadores de modelos predeterminados admiten la mayoría de los tipos de datos de .NET Core comunes y deberían cubrir las necesidades de casi cualquier desarrollador.</span><span class="sxs-lookup"><span data-stu-id="b1354-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="b1354-111">Esperan enlazar entradas basadas en texto desde la solicitud directamente a tipos de modelos.</span><span class="sxs-lookup"><span data-stu-id="b1354-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="b1354-112">Puede que sea necesario transformar la entrada antes de enlazarla.</span><span class="sxs-lookup"><span data-stu-id="b1354-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="b1354-113">Es el caso, por ejemplo, si tiene una clave que se puede usar para buscar datos del modelo.</span><span class="sxs-lookup"><span data-stu-id="b1354-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="b1354-114">Se puede usar un enlazador de modelos personalizado para capturar datos en función de la clave.</span><span class="sxs-lookup"><span data-stu-id="b1354-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="b1354-115">Revisión del enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="b1354-115">Model binding review</span></span>

<span data-ttu-id="b1354-116">El enlace de modelos usa definiciones específicas de los tipos con los que funciona.</span><span class="sxs-lookup"><span data-stu-id="b1354-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="b1354-117">Un *tipo simple* se convierte a partir de una sola cadena de la entrada,</span><span class="sxs-lookup"><span data-stu-id="b1354-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="b1354-118">mientras que un *tipo complejo* se convierte a partir de varios valores de entrada.</span><span class="sxs-lookup"><span data-stu-id="b1354-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="b1354-119">El marco establece la diferencia dependiendo de si existe un `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="b1354-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="b1354-120">Conviene crear un convertidor de tipos si existe una asignación simple `string` -> `SomeType` que no necesita recursos externos.</span><span class="sxs-lookup"><span data-stu-id="b1354-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="b1354-121">Antes de crear su propio enlazador de modelos personalizado, no está de más que repase cómo se implementan los enlazadores de modelos existentes.</span><span class="sxs-lookup"><span data-stu-id="b1354-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="b1354-122">Hay que considerar el uso de [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder), que sirve para convertir cadenas codificadas con base64 en matrices de bytes.</span><span class="sxs-lookup"><span data-stu-id="b1354-122">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="b1354-123">Las matrices de bytes se suelen almacenar como archivos o como campos de tipo BLOB de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b1354-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="b1354-124">Trabajar con ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="b1354-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="b1354-125">Las cadenas codificadas con base64 se pueden usar para representar datos binarios.</span><span class="sxs-lookup"><span data-stu-id="b1354-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="b1354-126">Por ejemplo, la siguiente imagen se puede codificar como una cadena.</span><span class="sxs-lookup"><span data-stu-id="b1354-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="b1354-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="b1354-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="b1354-128">En la siguiente imagen se muestra una pequeña porción de la cadena codificada:</span><span class="sxs-lookup"><span data-stu-id="b1354-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="b1354-129">![dotnet bot codificada](custom-model-binding/images/encoded-bot.png "dotnet bot codificada")</span><span class="sxs-lookup"><span data-stu-id="b1354-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="b1354-130">Siga las instrucciones del [archivo Léame del ejemplo](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) para convertir la cadena codificada con base64 en un archivo.</span><span class="sxs-lookup"><span data-stu-id="b1354-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="b1354-131">ASP.NET Core MVC toma cadenas codificadas con base64 y usa un `ByteArrayModelBinder` para convertirlas en una matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="b1354-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="b1354-132">[ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider), que implementa [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider), asigna argumentos `byte[]` a `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="b1354-132">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="b1354-133">Cuando cree su propio enlazador de modelos personalizado, puede implementar su tipo `IModelBinderProvider` particular o usar [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="b1354-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="b1354-134">En el siguiente ejemplo se indica cómo usar `ByteArrayModelBinder` para convertir una cadena codificada con base64 en un `byte[]` y guardar el resultado en un archivo:</span><span class="sxs-lookup"><span data-stu-id="b1354-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="b1354-135">Se puede usar un método API POST en una cadena codificada con base64 con una herramienta como [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="b1354-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="b1354-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="b1354-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="b1354-137">Siempre y cuando el enlazador pueda enlazar datos de la solicitud a argumentos o propiedades con el nombre adecuado, el enlace de modelos se realizará correctamente.</span><span class="sxs-lookup"><span data-stu-id="b1354-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="b1354-138">En el siguiente ejemplo se muestra cómo usar `ByteArrayModelBinder` con un modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="b1354-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="b1354-139">Ejemplo de enlazador de modelos personalizado</span><span class="sxs-lookup"><span data-stu-id="b1354-139">Custom model binder sample</span></span>

<span data-ttu-id="b1354-140">En esta sección implementaremos un enlazador de modelos personalizado que haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b1354-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="b1354-141">Convertir los datos de solicitud entrantes en argumentos clave fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="b1354-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="b1354-142">Usar Entity Framework Core para capturar la entidad asociada</span><span class="sxs-lookup"><span data-stu-id="b1354-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="b1354-143">Pasar la entidad asociada como argumento al método de acción</span><span class="sxs-lookup"><span data-stu-id="b1354-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="b1354-144">En el siguiente ejemplo se usa el atributo `ModelBinder` en el modelo `Author`:</span><span class="sxs-lookup"><span data-stu-id="b1354-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="b1354-145">En el código anterior, el atributo `ModelBinder` especifica el tipo de `IModelBinder` que se debe emplear para enlazar parámetros de acción de `Author`.</span><span class="sxs-lookup"><span data-stu-id="b1354-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="b1354-146">`AuthorEntityBinder` sirve para enlazar un parámetro `Author` capturando la entidad de un origen de datos por medio de Entity Framework Core y de un `authorId`:</span><span class="sxs-lookup"><span data-stu-id="b1354-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="b1354-147">En el siguiente código se indica cómo usar `AuthorEntityBinder` en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="b1354-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="b1354-148">El atributo `ModelBinder` se puede usar para aplicar `AuthorEntityBinder` a los parámetros que no usan convenciones predeterminadas:</span><span class="sxs-lookup"><span data-stu-id="b1354-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="b1354-149">En este ejemplo, como el nombre del argumento no es el `authorId` predeterminado, se especifica en el parámetro por medio del atributo `ModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="b1354-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="b1354-150">Observe que tanto el controlador como el método de acción se simplifican, en contraste con tener que buscar la entidad en el método de acción.</span><span class="sxs-lookup"><span data-stu-id="b1354-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="b1354-151">La lógica para capturar el autor a través de Entity Framework Core se traslada al enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="b1354-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="b1354-152">Esto puede reducir enormemente la complejidad cuando existen varios métodos que se enlazan con el modelo de autor, además de contribuir a seguir el [principio Una vez y solo una (DRY)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="b1354-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="b1354-153">Puede aplicar el atributo `ModelBinder` a propiedades de modelo individuales (como en un modelo de vista) o a parámetros del método de acción para especificar un determinado nombre de modelo o enlazador de modelos que sea exclusivo de ese tipo o acción en particular.</span><span class="sxs-lookup"><span data-stu-id="b1354-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="b1354-154">Implementar un ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="b1354-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="b1354-155">En lugar de aplicar un atributo, puede implementar `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="b1354-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="b1354-156">Así es como se implementan los enlazadores de marco integrados.</span><span class="sxs-lookup"><span data-stu-id="b1354-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="b1354-157">Cuando se especifica el tipo con el que el enlazador funciona, lo que se está indicando es el tipo de argumento que ese enlazador genera, **no** la entrada que acepta.</span><span class="sxs-lookup"><span data-stu-id="b1354-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="b1354-158">El siguiente proveedor de enlazador funciona con `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="b1354-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="b1354-159">Cuando se agrega a la colección de proveedores de MVC, no es necesario usar el atributo `ModelBinder` en los parámetros con tipo `Author` o `Author`.</span><span class="sxs-lookup"><span data-stu-id="b1354-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="b1354-160">Nota: El código anterior devuelve un `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="b1354-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="b1354-161">`BinderTypeModelBinder` actúa como una fábrica de enlazadores de modelos y proporciona la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b1354-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="b1354-162">`AuthorEntityBinder` requiere que la inserción de dependencias tenga acceso a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b1354-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="b1354-163">Use `BinderTypeModelBinder` si su enlazador de modelos necesita servicios de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b1354-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="b1354-164">Para usar un proveedor de enlazadores de modelos personalizado, agréguelo a `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b1354-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="b1354-165">Al evaluar enlazadores de modelos, la colección de proveedores se examina en orden.</span><span class="sxs-lookup"><span data-stu-id="b1354-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="b1354-166">Se usará el primer proveedor que devuelva un enlazador.</span><span class="sxs-lookup"><span data-stu-id="b1354-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="b1354-167">En la siguiente imagen se muestran los enlazadores de modelos predeterminados en el depurador.</span><span class="sxs-lookup"><span data-stu-id="b1354-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="b1354-168">![enlazadores de modelos predeterminados](custom-model-binding/images/default-model-binders.png "enlazadores de modelos predeterminados")</span><span class="sxs-lookup"><span data-stu-id="b1354-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="b1354-169">Si su proveedor se agrega al final de la colección, puede ocurrir que se llame a un enlazador de modelos integrado antes que al suyo.</span><span class="sxs-lookup"><span data-stu-id="b1354-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="b1354-170">En este ejemplo, el proveedor personalizado se agrega al principio de la colección para procurar que se use en los argumentos de acción de `Author`.</span><span class="sxs-lookup"><span data-stu-id="b1354-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="b1354-171">Sugerencias y procedimientos recomendados</span><span class="sxs-lookup"><span data-stu-id="b1354-171">Recommendations and best practices</span></span>

<span data-ttu-id="b1354-172">Los enlazadores de modelos personalizados deben caracterizarse por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b1354-172">Custom model binders:</span></span>
- <span data-ttu-id="b1354-173">No deben tratar de establecer códigos de estado ni devolver resultados (por ejemplo, 404 No encontrado).</span><span class="sxs-lookup"><span data-stu-id="b1354-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="b1354-174">Los errores que se produzcan en un enlace de modelos se deben controlar con un [filtro de acciones](xref:mvc/controllers/filters) o con la lógica del propio método de acción.</span><span class="sxs-lookup"><span data-stu-id="b1354-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="b1354-175">Son realmente útiles para eliminar el código repetitivo y las cuestiones transversales de los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="b1354-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="b1354-176">No se deben usar en general para convertir una cadena en un tipo personalizado. Para ello, [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) suele ser una mejor opción.</span><span class="sxs-lookup"><span data-stu-id="b1354-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
