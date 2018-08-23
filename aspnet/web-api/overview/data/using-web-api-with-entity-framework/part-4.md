---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Controlar las relaciones de entidad | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 5d05a2e5d4380a15078317545325bd20fde3f83c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838525"
---
<a name="handling-entity-relations"></a><span data-ttu-id="66e54-102">Relaciones de entidad de control</span><span class="sxs-lookup"><span data-stu-id="66e54-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="66e54-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="66e54-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="66e54-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="66e54-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="66e54-105">Esta sección describen algunos detalles de cómo EF carga las entidades relacionadas y cómo controlar las propiedades de navegación circular en las clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="66e54-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="66e54-106">(Esta sección proporciona los conocimientos y no es necesario para completar el tutorial.</span><span class="sxs-lookup"><span data-stu-id="66e54-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="66e54-107">Si lo prefiere, vaya a [parte 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="66e54-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="66e54-108">Diligente frente a la carga diferida</span><span class="sxs-lookup"><span data-stu-id="66e54-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="66e54-109">Cuando se usa EF con una base de datos relacional, es importante comprender cómo EF carga los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="66e54-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="66e54-110">También es útil ver las consultas SQL que genera EF.</span><span class="sxs-lookup"><span data-stu-id="66e54-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="66e54-111">Para realizar el seguimiento de SQL, agregue la siguiente línea de código para el `BookServiceContext` constructor:</span><span class="sxs-lookup"><span data-stu-id="66e54-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="66e54-112">Si envía una solicitud GET a /api/books, devuelve JSON similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="66e54-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="66e54-113">Puede ver que la propiedad Author es null, aunque el libro contiene un AuthorId válido.</span><span class="sxs-lookup"><span data-stu-id="66e54-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="66e54-114">Eso es porque EF no carga las entidades relacionadas del autor.</span><span class="sxs-lookup"><span data-stu-id="66e54-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="66e54-115">Esto confirma que el registro de seguimiento de la consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="66e54-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="66e54-116">La instrucción SELECT toma de la tabla de los libros en pantalla y no hace referencia a la tabla de autor.</span><span class="sxs-lookup"><span data-stu-id="66e54-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="66e54-117">Como referencia, este es el método en el `BooksController` clase que devuelve la lista de los libros en pantalla.</span><span class="sxs-lookup"><span data-stu-id="66e54-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="66e54-118">Veamos cómo podemos volver al autor como parte de los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="66e54-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="66e54-119">Hay tres formas de cargar datos relacionados en Entity Framework: la carga diligente, la carga diferida y la carga explícita.</span><span class="sxs-lookup"><span data-stu-id="66e54-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="66e54-120">Existen ventajas y desventajas con cada una de ellas, por lo que es importante comprender cómo funcionan.</span><span class="sxs-lookup"><span data-stu-id="66e54-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="66e54-121">Carga diligente</span><span class="sxs-lookup"><span data-stu-id="66e54-121">Eager Loading</span></span>

<span data-ttu-id="66e54-122">Con *carga diligente*, EF carga las entidades relacionadas como parte de la consulta de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="66e54-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="66e54-123">Para realizar la carga diligente, utilice el **System.Data.Entity.Include** método de extensión.</span><span class="sxs-lookup"><span data-stu-id="66e54-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="66e54-124">Esto indica a EF para incluir los datos del autor de la consulta.</span><span class="sxs-lookup"><span data-stu-id="66e54-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="66e54-125">Si realiza este cambio y ejecutar la aplicación, ahora los datos JSON tendrá este aspecto:</span><span class="sxs-lookup"><span data-stu-id="66e54-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="66e54-126">El registro de seguimiento muestra que EF realiza una combinación en las tablas del libro y autor.</span><span class="sxs-lookup"><span data-stu-id="66e54-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="66e54-127">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="66e54-127">Lazy Loading</span></span>

<span data-ttu-id="66e54-128">Con la carga diferida, EF carga automáticamente una entidad relacionada cuando la propiedad de navegación para esa entidad se desreferencia.</span><span class="sxs-lookup"><span data-stu-id="66e54-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="66e54-129">Para habilitar la carga diferida, que la propiedad de navegación virtual.</span><span class="sxs-lookup"><span data-stu-id="66e54-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="66e54-130">Por ejemplo, en la clase Book:</span><span class="sxs-lookup"><span data-stu-id="66e54-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="66e54-131">Ahora, considere el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="66e54-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="66e54-132">Cuando se habilita la carga diferida, obtener acceso a la `Author` propiedad `books[0]` hace que EF consultar la base de datos para el autor.</span><span class="sxs-lookup"><span data-stu-id="66e54-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="66e54-133">La carga diferida requiere varios viajes de base de datos, dado que EF envía una consulta cada vez que recupera una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="66e54-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="66e54-134">Por lo general, desea deshabilitar para los objetos que se serializa la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="66e54-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="66e54-135">El serializador tiene que leer todas las propiedades en el modelo, que se desencadena al cargar las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="66e54-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="66e54-136">Por ejemplo, presentamos las consultas SQL que EF serializa la lista de libros con la carga diferida habilitada.</span><span class="sxs-lookup"><span data-stu-id="66e54-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="66e54-137">Puede ver que EF realiza tres consultas independientes para los autores de tres.</span><span class="sxs-lookup"><span data-stu-id="66e54-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="66e54-138">Todavía hay veces cuando desea utilizar la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="66e54-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="66e54-139">Para hacer que la carga diligente EF generar una combinación muy compleja.</span><span class="sxs-lookup"><span data-stu-id="66e54-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="66e54-140">O podría necesitar las entidades relacionadas para un pequeño subconjunto de los datos y la carga diferida sería más eficaz.</span><span class="sxs-lookup"><span data-stu-id="66e54-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="66e54-141">Es una forma de evitar problemas de serialización serializar objetos de transferencia de datos (dto) en lugar de objetos de entidad.</span><span class="sxs-lookup"><span data-stu-id="66e54-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="66e54-142">Este enfoque le mostraré más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="66e54-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="66e54-143">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="66e54-143">Explicit Loading</span></span>

<span data-ttu-id="66e54-144">Carga explícita es similar a la carga diferida, salvo que obtener explícitamente los datos relacionados en el código; no sucede automáticamente cuando tiene acceso a una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="66e54-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="66e54-145">Carga explícita le ofrece más control sobre cuándo se debe cargar los datos relacionados, pero requiere código adicional.</span><span class="sxs-lookup"><span data-stu-id="66e54-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="66e54-146">Para obtener más información acerca de la carga explícita, vea [cargar entidades relacionadas](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="66e54-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="66e54-147">Las propiedades de navegación y las referencias circulares</span><span class="sxs-lookup"><span data-stu-id="66e54-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="66e54-148">Cuando definen los modelos de libro y autor, definí una propiedad de navegación en el `Book` clase para la relación del autor del libro, pero no he definido una propiedad de navegación en la otra dirección.</span><span class="sxs-lookup"><span data-stu-id="66e54-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="66e54-149">¿Qué ocurre si agrega la propiedad de navegación correspondiente a la `Author` clase?</span><span class="sxs-lookup"><span data-stu-id="66e54-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="66e54-150">Lamentablemente, esto crea un problema al serializar los modelos.</span><span class="sxs-lookup"><span data-stu-id="66e54-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="66e54-151">Si carga los datos relacionados, crea un gráfico circular de objeto.</span><span class="sxs-lookup"><span data-stu-id="66e54-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="66e54-152">Cuando se intente el formateador JSON o XML serializar el gráfico, producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="66e54-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="66e54-153">Los dos formateadores generar mensajes de excepción diferente.</span><span class="sxs-lookup"><span data-stu-id="66e54-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="66e54-154">Este es un ejemplo para el formateador JSON:</span><span class="sxs-lookup"><span data-stu-id="66e54-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="66e54-155">Este es el formateador XML:</span><span class="sxs-lookup"><span data-stu-id="66e54-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="66e54-156">Una solución consiste en usar dto, que describe en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="66e54-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="66e54-157">Como alternativa, puede configurar los formateadores JSON y XML para controlar los ciclos de gráfico.</span><span class="sxs-lookup"><span data-stu-id="66e54-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="66e54-158">Para obtener más información, consulte [referencias a objetos de control Circular](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="66e54-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="66e54-159">Para este tutorial, no necesita la `Author.Book` propiedad de navegación, por lo que puede dejarlo.</span><span class="sxs-lookup"><span data-stu-id="66e54-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="66e54-160">[Anterior](part-3.md)
> [Siguiente](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="66e54-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
