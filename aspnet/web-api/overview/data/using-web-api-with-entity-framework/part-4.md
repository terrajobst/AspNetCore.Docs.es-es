---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Control de relaciones de entidad | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872694"
---
<a name="handling-entity-relations"></a><span data-ttu-id="86108-102">Relaciones de entidad de control</span><span class="sxs-lookup"><span data-stu-id="86108-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="86108-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="86108-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="86108-104">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="86108-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="86108-105">En esta sección se describe algunos detalles de cómo EF carga las entidades relacionadas y cómo controlar las propiedades de navegación circular en las clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="86108-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="86108-106">(Esta sección ofrece información de segundo plano y no es necesario para completar el tutorial.</span><span class="sxs-lookup"><span data-stu-id="86108-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="86108-107">Si lo prefiere, vaya a [parte 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="86108-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="86108-108">Carga frente a la carga diferida diligente</span><span class="sxs-lookup"><span data-stu-id="86108-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="86108-109">Al usar EF con una base de datos relacional, es importante entender la forma en que EF carga los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="86108-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="86108-110">También es útil ver las consultas SQL que genera EF.</span><span class="sxs-lookup"><span data-stu-id="86108-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="86108-111">Para realizar el seguimiento de SQL, agregue la siguiente línea de código para el `BookServiceContext` constructor:</span><span class="sxs-lookup"><span data-stu-id="86108-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="86108-112">Si se envía una solicitud GET a /api/books, devuelve JSON similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="86108-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="86108-113">Puede ver que la propiedad Author es null, incluso si el libro contiene un AuthorId válido.</span><span class="sxs-lookup"><span data-stu-id="86108-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="86108-114">Eso es porque EF no está cargando las entidades relacionadas de autor.</span><span class="sxs-lookup"><span data-stu-id="86108-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="86108-115">Esto confirma que el registro de seguimiento de la consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="86108-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="86108-116">La instrucción SELECT toma de la tabla de libros y no hace referencia a la tabla de autor.</span><span class="sxs-lookup"><span data-stu-id="86108-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="86108-117">Como referencia, éste es el método en el `BooksController` clase que devuelve la lista de libros.</span><span class="sxs-lookup"><span data-stu-id="86108-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="86108-118">Veamos cómo podemos devolvemos al autor como parte de los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="86108-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="86108-119">Hay tres maneras de cargar los datos relacionados en Entity Framework: carga diligente, la carga diferida y carga explícita.</span><span class="sxs-lookup"><span data-stu-id="86108-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="86108-120">Hay ventajas y desventajas con cada una de ellas, por lo que es importante comprender cómo funcionan.</span><span class="sxs-lookup"><span data-stu-id="86108-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="86108-121">Carga diligente</span><span class="sxs-lookup"><span data-stu-id="86108-121">Eager Loading</span></span>

<span data-ttu-id="86108-122">Con *carga diligente*, EF carga las entidades relacionadas como parte de la consulta de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="86108-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="86108-123">Para realizar la carga diligente, utilice la **System.Data.Entity.Include** método de extensión.</span><span class="sxs-lookup"><span data-stu-id="86108-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="86108-124">Esto indica a EF para incluir los datos de autor en la consulta.</span><span class="sxs-lookup"><span data-stu-id="86108-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="86108-125">Si realiza este cambio y ejecutar la aplicación, ahora los datos JSON tendrá este aspecto:</span><span class="sxs-lookup"><span data-stu-id="86108-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="86108-126">El registro de seguimiento muestra que EF realiza una combinación en las tablas del libro y el autor.</span><span class="sxs-lookup"><span data-stu-id="86108-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="86108-127">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="86108-127">Lazy Loading</span></span>

<span data-ttu-id="86108-128">Con la carga diferida, EF carga automáticamente una entidad relacionada cuando se deshace la referencia de la propiedad de navegación para esa entidad.</span><span class="sxs-lookup"><span data-stu-id="86108-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="86108-129">Para habilitar la carga diferida, convierta la propiedad de navegación virtual.</span><span class="sxs-lookup"><span data-stu-id="86108-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="86108-130">Por ejemplo, en la clase de libro:</span><span class="sxs-lookup"><span data-stu-id="86108-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="86108-131">Ahora, considere el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="86108-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="86108-132">Cuando está habilitada la carga diferida, obteniendo acceso a la `Author` propiedad `books[0]` hace EF consultar la base de datos para el autor.</span><span class="sxs-lookup"><span data-stu-id="86108-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="86108-133">Carga diferida requiere varios viajes de base de datos, porque EF envía una consulta cada vez que recupera una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="86108-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="86108-134">En general, es conveniente deshabilitada para los objetos que se serializa la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="86108-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="86108-135">El serializador tiene que leer todas las propiedades en el modelo, lo que desencadena la carga de las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="86108-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="86108-136">Por ejemplo, cuando estas son las consultas SQL EF serializa la lista de libros con carga diferida habilitada.</span><span class="sxs-lookup"><span data-stu-id="86108-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="86108-137">Puede ver que EF hace tres consultas independientes para los autores de tres.</span><span class="sxs-lookup"><span data-stu-id="86108-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="86108-138">Hay veces cuando desea usar la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="86108-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="86108-139">Carga diligente puede provocar EF generar una combinación muy compleja.</span><span class="sxs-lookup"><span data-stu-id="86108-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="86108-140">O podría necesitar las entidades relacionadas para un pequeño subconjunto de los datos y la carga diferida sería mucho más eficaz.</span><span class="sxs-lookup"><span data-stu-id="86108-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="86108-141">Es una manera de evitar problemas de serialización serializar objetos de transferencia de datos (dto) en lugar de objetos de entidad.</span><span class="sxs-lookup"><span data-stu-id="86108-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="86108-142">Voy a explicar este enfoque más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="86108-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="86108-143">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="86108-143">Explicit Loading</span></span>

<span data-ttu-id="86108-144">Carga explícita es similar a la carga diferida, salvo que obtener explícitamente los datos relacionados en el código. no sucede automáticamente cuando tiene acceso a una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="86108-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="86108-145">Carga explícita ofrece un mayor control sobre cuándo se debe cargar los datos relacionados, pero requiere código adicional.</span><span class="sxs-lookup"><span data-stu-id="86108-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="86108-146">Para obtener más información acerca de la carga explícita, vea [cargar entidades relacionadas](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="86108-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="86108-147">Propiedades de navegación y las referencias circulares</span><span class="sxs-lookup"><span data-stu-id="86108-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="86108-148">Cuando definen los modelos del libro y el autor, definir una propiedad de navegación en la `Book` clase para la relación del autor del libro, pero no ha definido una propiedad de navegación en la otra dirección.</span><span class="sxs-lookup"><span data-stu-id="86108-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="86108-149">¿Qué ocurre si agrega la propiedad de navegación correspondiente a la `Author` clase?</span><span class="sxs-lookup"><span data-stu-id="86108-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="86108-150">Por desgracia, surge un problema al serializar los modelos.</span><span class="sxs-lookup"><span data-stu-id="86108-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="86108-151">Si carga los datos relacionados, crea un gráfico de objetos circulares.</span><span class="sxs-lookup"><span data-stu-id="86108-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="86108-152">Cuando el formateador XML o JSON intenta serializar el gráfico, producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="86108-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="86108-153">Los dos formateadores producen mensajes de excepción diferentes.</span><span class="sxs-lookup"><span data-stu-id="86108-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="86108-154">Este es un ejemplo para el formateador JSON:</span><span class="sxs-lookup"><span data-stu-id="86108-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="86108-155">Este es el formateador de XML:</span><span class="sxs-lookup"><span data-stu-id="86108-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="86108-156">Una solución consiste en usar dto, que describe en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="86108-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="86108-157">Como alternativa, puede configurar los formateadores JSON y XML para controlar los ciclos de gráfico.</span><span class="sxs-lookup"><span data-stu-id="86108-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="86108-158">Para obtener más información, consulte [control Circular referencias a objetos](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="86108-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="86108-159">Para este tutorial, no es necesario el `Author.Book` propiedad de navegación, por lo que puede dejar.</span><span class="sxs-lookup"><span data-stu-id="86108-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86108-160">[Anterior](part-3.md)
> [Siguiente](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="86108-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
