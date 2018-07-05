---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Crear objetos de transferencia de datos (dto) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 6a066f1aca909afc2956e2026d9025cb87f10b1f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399930"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="d5263-102">Crear objetos de transferencia de datos (dto)</span><span class="sxs-lookup"><span data-stu-id="d5263-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="d5263-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d5263-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d5263-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="d5263-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d5263-105">Derecha ahora, nuestra API de web expone las entidades de base de datos al cliente.</span><span class="sxs-lookup"><span data-stu-id="d5263-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="d5263-106">El cliente recibe los datos que se asigna directamente a las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d5263-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="d5263-107">Sin embargo, no siempre es una buena idea.</span><span class="sxs-lookup"><span data-stu-id="d5263-107">However, that's not always a good idea.</span></span> <span data-ttu-id="d5263-108">A veces desea cambiar la forma de los datos que envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="d5263-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="d5263-109">Por ejemplo, puedes:</span><span class="sxs-lookup"><span data-stu-id="d5263-109">For example, you might want to:</span></span>

- <span data-ttu-id="d5263-110">Quite las referencias circulares (consulte la sección anterior).</span><span class="sxs-lookup"><span data-stu-id="d5263-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="d5263-111">Ocultar determinadas propiedades que los clientes no deberían para ver.</span><span class="sxs-lookup"><span data-stu-id="d5263-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="d5263-112">Omitir algunas de las propiedades con el fin de reducir el tamaño de la carga.</span><span class="sxs-lookup"><span data-stu-id="d5263-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="d5263-113">Eliminar el formato de gráficos de objetos que contienen objetos anidados, para que sean más conveniente para los clientes.</span><span class="sxs-lookup"><span data-stu-id="d5263-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="d5263-114">Evite "exceso" las vulnerabilidades por publicación.</span><span class="sxs-lookup"><span data-stu-id="d5263-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="d5263-115">(Consulte [validación del modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para obtener una explicación de la publicación excesiva.)</span><span class="sxs-lookup"><span data-stu-id="d5263-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="d5263-116">Desacoplar del nivel de servicio de su capa de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d5263-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="d5263-117">Para lograr esto, puede definir un *objeto de transferencia de datos* (DTO).</span><span class="sxs-lookup"><span data-stu-id="d5263-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="d5263-118">Un DTO es un objeto que define cómo se enviarán los datos a través de la red.</span><span class="sxs-lookup"><span data-stu-id="d5263-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="d5263-119">Veamos cómo funciona con la entidad del libro.</span><span class="sxs-lookup"><span data-stu-id="d5263-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="d5263-120">En la carpeta Models, agregue dos clases DTO:</span><span class="sxs-lookup"><span data-stu-id="d5263-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="d5263-121">El `BookDetailDTO` clase incluye todas las propiedades del modelo de libro, salvo que `AuthorName` es una cadena que se va a contener el nombre del autor.</span><span class="sxs-lookup"><span data-stu-id="d5263-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="d5263-122">El `BookDTO` clase contiene un subconjunto de propiedades de `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="d5263-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="d5263-123">A continuación, reemplace los dos métodos GET en el `BooksController` (clase), con las versiones que devuelven objetos dto.</span><span class="sxs-lookup"><span data-stu-id="d5263-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="d5263-124">Vamos a usar LINQ **seleccione** instrucción para convertir de entidades de libro en el dto.</span><span class="sxs-lookup"><span data-stu-id="d5263-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="d5263-125">Este es el SQL generado por el nuevo `GetBooks` método.</span><span class="sxs-lookup"><span data-stu-id="d5263-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="d5263-126">Puede ver que EF traduce el LINQ **seleccione** en una instrucción SELECT de SQL.</span><span class="sxs-lookup"><span data-stu-id="d5263-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="d5263-127">Por último, modifique el `PostBook` método devuelva un DTO.</span><span class="sxs-lookup"><span data-stu-id="d5263-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="d5263-128">En este tutorial, estamos convirtiendo los dto manualmente en el código.</span><span class="sxs-lookup"><span data-stu-id="d5263-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="d5263-129">Otra opción consiste en usar una biblioteca como [AutoMapper](http://automapper.org/) que controla la conversión automáticamente.</span><span class="sxs-lookup"><span data-stu-id="d5263-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="d5263-130">[Anterior](part-4.md)
> [Siguiente](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="d5263-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
