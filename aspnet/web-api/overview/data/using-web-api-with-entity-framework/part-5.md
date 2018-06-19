---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Crear objetos de transferencia de datos (dto) | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878690"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="98240-102">Crear objetos de transferencia de datos (dto)</span><span class="sxs-lookup"><span data-stu-id="98240-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="98240-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="98240-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="98240-104">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="98240-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="98240-105">Derecha, nuestra API web expone las entidades de base de datos al cliente.</span><span class="sxs-lookup"><span data-stu-id="98240-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="98240-106">El cliente recibe los datos que se asigna directamente a las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="98240-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="98240-107">Sin embargo, no siempre es una buena idea.</span><span class="sxs-lookup"><span data-stu-id="98240-107">However, that's not always a good idea.</span></span> <span data-ttu-id="98240-108">A veces desea cambiar la forma de los datos que envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="98240-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="98240-109">Por ejemplo, puedes:</span><span class="sxs-lookup"><span data-stu-id="98240-109">For example, you might want to:</span></span>

- <span data-ttu-id="98240-110">Quite las referencias circulares (vea la sección anterior).</span><span class="sxs-lookup"><span data-stu-id="98240-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="98240-111">Ocultar determinadas propiedades que los clientes no deberían para ver.</span><span class="sxs-lookup"><span data-stu-id="98240-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="98240-112">Omitir algunas propiedades para reducir el tamaño de carga.</span><span class="sxs-lookup"><span data-stu-id="98240-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="98240-113">Recopila los gráficos de objetos que contienen objetos anidados, para que sean más cómodo para los clientes.</span><span class="sxs-lookup"><span data-stu-id="98240-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="98240-114">Evite "contabilización exceso" vulnerabilidades.</span><span class="sxs-lookup"><span data-stu-id="98240-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="98240-115">(Consulte [validación del modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para obtener una explicación del registro excesivo.)</span><span class="sxs-lookup"><span data-stu-id="98240-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="98240-116">Desacoplar el nivel de servicio de la capa de base de datos.</span><span class="sxs-lookup"><span data-stu-id="98240-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="98240-117">Para ello, puede definir un *objeto de transferencia de datos* (DTO).</span><span class="sxs-lookup"><span data-stu-id="98240-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="98240-118">Un DTO es un objeto que define cómo se enviará los datos a través de la red.</span><span class="sxs-lookup"><span data-stu-id="98240-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="98240-119">Veamos cómo funciona con la entidad del libro.</span><span class="sxs-lookup"><span data-stu-id="98240-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="98240-120">En la carpeta Models, agregue dos clases DTO:</span><span class="sxs-lookup"><span data-stu-id="98240-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="98240-121">El `BookDetailDTO` clase incluye todas las propiedades del modelo del libro, salvo que `AuthorName` es una cadena que contendrá el nombre del autor.</span><span class="sxs-lookup"><span data-stu-id="98240-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="98240-122">El `BookDTO` clase contiene un subconjunto de propiedades de `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="98240-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="98240-123">A continuación, reemplace los dos métodos GET en el `BooksController` (clase), con las versiones que devuelven dto.</span><span class="sxs-lookup"><span data-stu-id="98240-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="98240-124">Vamos a usar LINQ **seleccione** instrucción que se va a convertir de entidades de libro en dto.</span><span class="sxs-lookup"><span data-stu-id="98240-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="98240-125">Este es el código SQL generado por el nuevo `GetBooks` método.</span><span class="sxs-lookup"><span data-stu-id="98240-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="98240-126">Puede ver que EF traduce LINQ **seleccione** en una instrucción SELECT de SQL.</span><span class="sxs-lookup"><span data-stu-id="98240-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="98240-127">Por último, modifique el `PostBook` método devuelva un DTO.</span><span class="sxs-lookup"><span data-stu-id="98240-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="98240-128">En este tutorial, nos estamos se convierte en dto manualmente en el código.</span><span class="sxs-lookup"><span data-stu-id="98240-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="98240-129">Otra opción consiste en utilizar una biblioteca como [AutoMapper](http://automapper.org/) que controla la conversión automáticamente.</span><span class="sxs-lookup"><span data-stu-id="98240-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="98240-130">[Anterior](part-4.md)
> [Siguiente](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="98240-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
