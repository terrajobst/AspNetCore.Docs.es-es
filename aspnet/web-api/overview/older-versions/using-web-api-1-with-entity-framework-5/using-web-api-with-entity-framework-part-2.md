---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: Crear modelos de dominio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f379c92f7795c9fe10f7860b72188a8cfc1b6d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379730"
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="21ffa-102">Parte 2: Crear modelos de dominio</span><span class="sxs-lookup"><span data-stu-id="21ffa-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="21ffa-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="21ffa-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="21ffa-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="21ffa-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="21ffa-105">Agregar modelos</span><span class="sxs-lookup"><span data-stu-id="21ffa-105">Add Models</span></span>

<span data-ttu-id="21ffa-106">Hay tres maneras de enfoque de Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="21ffa-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="21ffa-107">Base de datos en primer lugar: empieza con una base de datos, y Entity Framework genera el código.</span><span class="sxs-lookup"><span data-stu-id="21ffa-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="21ffa-108">Model first: empieza con un modelo visual, y Entity Framework genera la base de datos y el código.</span><span class="sxs-lookup"><span data-stu-id="21ffa-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="21ffa-109">Code first: empieza con el código, y Entity Framework genera la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21ffa-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="21ffa-110">Estamos usando el enfoque de code first, por lo que comenzamos por definir nuestros objetos de dominio como poco (objetos CLR antiguos sin formato).</span><span class="sxs-lookup"><span data-stu-id="21ffa-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="21ffa-111">Con el enfoque de code first, objetos de dominio no es necesario ningún código adicional para admitir la capa de base de datos, como las transacciones ni la persistencia.</span><span class="sxs-lookup"><span data-stu-id="21ffa-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="21ffa-112">(En concreto, no es necesario que se heredará la [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) clase.) Todavía puede usar anotaciones de datos para controlar cómo Entity Framework crea el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="21ffa-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="21ffa-113">Dado que poco no transportan las propiedades adicionales que describen [estado de la base de datos](https://msdn.microsoft.com/library/system.data.entitystate.aspx), fácilmente se pueden serializar a JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="21ffa-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="21ffa-114">Sin embargo, eso no significa que siempre deben exponer los modelos de Entity Framework directamente a los clientes, como veremos más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="21ffa-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="21ffa-115">Vamos a crear los objetos poco siguientes:</span><span class="sxs-lookup"><span data-stu-id="21ffa-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="21ffa-116">Producto</span><span class="sxs-lookup"><span data-stu-id="21ffa-116">Product</span></span>
- <span data-ttu-id="21ffa-117">Orden</span><span class="sxs-lookup"><span data-stu-id="21ffa-117">Order</span></span>
- <span data-ttu-id="21ffa-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="21ffa-118">OrderDetail</span></span>

<span data-ttu-id="21ffa-119">Para crear cada clase, haga clic en la carpeta de modelos en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="21ffa-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="21ffa-120">En el menú contextual, seleccione **agregar** y, a continuación, seleccione **clase.**</span><span class="sxs-lookup"><span data-stu-id="21ffa-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="21ffa-121">Agregar un `Product` clase con la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="21ffa-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="21ffa-122">Por convención, Entity Framework usa el `Id` propiedad como clave principal y lo asigna a una columna de identidad en la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="21ffa-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="21ffa-123">Cuando se crea un nuevo `Product` instancia, no establezca un valor para `Id`, porque la base de datos genera el valor.</span><span class="sxs-lookup"><span data-stu-id="21ffa-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="21ffa-124">El **ScaffoldColumn** atributo indica a ASP.NET MVC para omitir la `Id` propiedad cuando se genera un formulario del editor.</span><span class="sxs-lookup"><span data-stu-id="21ffa-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="21ffa-125">El **necesario** atributo se utiliza para validar el modelo.</span><span class="sxs-lookup"><span data-stu-id="21ffa-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="21ffa-126">Especifica que el `Name` propiedad debe ser una cadena no vacía.</span><span class="sxs-lookup"><span data-stu-id="21ffa-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="21ffa-127">Agregar el `Order` clase:</span><span class="sxs-lookup"><span data-stu-id="21ffa-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="21ffa-128">Agregar el `OrderDetail` clase:</span><span class="sxs-lookup"><span data-stu-id="21ffa-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="21ffa-129">Relaciones de clave externas</span><span class="sxs-lookup"><span data-stu-id="21ffa-129">Foreign Key Relations</span></span>

<span data-ttu-id="21ffa-130">Un pedido contiene numerosos detalles de pedido y cada detalle de pedido hace referencia a un solo producto.</span><span class="sxs-lookup"><span data-stu-id="21ffa-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="21ffa-131">Para representar estas relaciones, la `OrderDetail` clase define propiedades denominadas `OrderId` y `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="21ffa-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="21ffa-132">Entity Framework deducirá que estas propiedades representan las claves externas y agregará las restricciones foreign key a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21ffa-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="21ffa-133">El `Order` y `OrderDetail` clases también incluyen propiedades de "exploración", que contienen referencias a los objetos relacionados.</span><span class="sxs-lookup"><span data-stu-id="21ffa-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="21ffa-134">Dado un pedido, puede navegar a los productos en el orden siguiendo las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="21ffa-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="21ffa-135">Compile el proyecto ahora.</span><span class="sxs-lookup"><span data-stu-id="21ffa-135">Compile the project now.</span></span> <span data-ttu-id="21ffa-136">Entity Framework usa la reflexión para detectar las propiedades de los modelos, por lo que requiere un ensamblado compilado crear el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="21ffa-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="21ffa-137">Configurar a los formateadores de tipo de medio</span><span class="sxs-lookup"><span data-stu-id="21ffa-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="21ffa-138">Un [formateador de tipo de medio](../../formats-and-model-binding/media-formatters.md) es un objeto que serializa los datos cuando la API Web escribe el cuerpo de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="21ffa-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="21ffa-139">Los formateadores integrados admiten la salida JSON y XML.</span><span class="sxs-lookup"><span data-stu-id="21ffa-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="21ffa-140">De forma predeterminada, ambos de estos formateadores serializar todos los objetos por valor.</span><span class="sxs-lookup"><span data-stu-id="21ffa-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="21ffa-141">Serialización por valor crea un problema si un gráfico de objetos contiene las referencias circulares.</span><span class="sxs-lookup"><span data-stu-id="21ffa-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="21ffa-142">Que es exactamente el caso de los `Order` y `OrderDetail` clases, porque cada uno contiene una referencia a otro.</span><span class="sxs-lookup"><span data-stu-id="21ffa-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="21ffa-143">El formateador se siguen las referencias, escribir cada objeto por valor e ir en círculos.</span><span class="sxs-lookup"><span data-stu-id="21ffa-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="21ffa-144">Por lo tanto, es necesario cambiar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="21ffa-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="21ffa-145">En el Explorador de soluciones, expanda la aplicación\_carpeta Inicio y abra el archivo WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="21ffa-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="21ffa-146">Agregue el código siguiente a la clase `WebApiConfig` :</span><span class="sxs-lookup"><span data-stu-id="21ffa-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="21ffa-147">Este código establece el formateador JSON para conservar las referencias de objeto y quita completamente el formateador XML de la canalización.</span><span class="sxs-lookup"><span data-stu-id="21ffa-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="21ffa-148">(Puede configurar el formateador XML para conservar las referencias de objeto, pero es un poco más trabajo y solo necesitamos JSON para esta aplicación.</span><span class="sxs-lookup"><span data-stu-id="21ffa-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="21ffa-149">Para obtener más información, consulte [referencias a objetos de control Circular](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="21ffa-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="21ffa-150">[Anterior](using-web-api-with-entity-framework-part-1.md)
> [Siguiente](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="21ffa-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
