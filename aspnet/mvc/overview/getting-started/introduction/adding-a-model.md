---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Adición de un modelo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 3b9d84ae757eac6cc2a1ae8f7857244adff50d7c
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578281"
---
<a name="adding-a-model"></a><span data-ttu-id="18a4b-102">Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="18a4b-102">Adding a Model</span></span>
====================
<span data-ttu-id="18a4b-103">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="18a4b-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="18a4b-104">En esta sección agregará algunas clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="18a4b-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="18a4b-105">Estas clases serán el &quot;modelo&quot; forma parte de la aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="18a4b-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="18a4b-106">Deberá usar una tecnología de acceso a datos de .NET Framework conocida como la [Entity Framework](https://docs.microsoft.com/ef/) para definir y trabajar con estas clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="18a4b-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="18a4b-107">El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*.</span><span class="sxs-lookup"><span data-stu-id="18a4b-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="18a4b-108">Código primero permite crear objetos de modelo mediante la escritura de clases simple.</span><span class="sxs-lookup"><span data-stu-id="18a4b-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="18a4b-109">(También conocido como son las clases POCO, de &quot;los objetos CLR antiguos sin formato.&quot;) A continuación, puede tener la base de datos que se crean sobre la marcha de las clases, lo que permite un flujo de trabajo de desarrollo muy limpio y rápido.</span><span class="sxs-lookup"><span data-stu-id="18a4b-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="18a4b-110">Si es necesario crear primero la base de datos, puede seguir este tutorial para aprender sobre el desarrollo de aplicaciones MVC y EF.</span><span class="sxs-lookup"><span data-stu-id="18a4b-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="18a4b-111">A continuación, puede seguir Tom Fizmakens [Scaffolding de ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, que cubre el primer enfoque de base de datos.</span><span class="sxs-lookup"><span data-stu-id="18a4b-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="18a4b-112">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="18a4b-112">Adding Model Classes</span></span>

<span data-ttu-id="18a4b-113">En **el Explorador de soluciones**, haga clic en el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="18a4b-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="18a4b-114">Escriba el *clase* nombre &quot;película&quot;.</span><span class="sxs-lookup"><span data-stu-id="18a4b-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="18a4b-115">Agregue las siguientes cinco propiedades para el `Movie` clase:</span><span class="sxs-lookup"><span data-stu-id="18a4b-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="18a4b-116">Vamos a usar la `Movie` clase para representar las películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="18a4b-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="18a4b-117">Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.</span><span class="sxs-lookup"><span data-stu-id="18a4b-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="18a4b-118">Nota: Para poder usar la clase relacionada y System.Data.Entity, deberá instalar el [paquete NuGet de Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="18a4b-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="18a4b-119">Siga el vínculo para obtener más instrucciones.</span><span class="sxs-lookup"><span data-stu-id="18a4b-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="18a4b-120">En el mismo archivo, agregue la siguiente `MovieDBContext` clase:</span><span class="sxs-lookup"><span data-stu-id="18a4b-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="18a4b-121">El `MovieDBContext` clase representa el contexto de base de datos de películas de Entity Framework, que se encarga de capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase.</span><span class="sxs-lookup"><span data-stu-id="18a4b-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="18a4b-122">El `MovieDBContext` deriva el `DbContext` proporcionado por Entity Framework de clase base.</span><span class="sxs-lookup"><span data-stu-id="18a4b-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="18a4b-123">Para poder hacer referencia a `DbContext` y `DbSet`, deberá agregar lo siguiente `using` instrucción en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="18a4b-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="18a4b-124">Puede hacerlo agregando manualmente el uso de la instrucción, o bien puede mantener el mouse sobre las líneas onduladas rojas, haga clic en `Show potential fixes` y haga clic en `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="18a4b-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="18a4b-125">Nota: Algunos sin usar `using` instrucciones se han quitado.</span><span class="sxs-lookup"><span data-stu-id="18a4b-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="18a4b-126">Visual Studio mostrará las dependencias no usadas en gris.</span><span class="sxs-lookup"><span data-stu-id="18a4b-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="18a4b-127">Puede quitar las dependencias no usadas manteniendo el mouse sobre las dependencias de gris, haga clic en `Show potential fixes` y haga clic en **quitar instrucciones using no usadas.**</span><span class="sxs-lookup"><span data-stu-id="18a4b-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="18a4b-128">Por último, agregamos un modelo (M en MVC).</span><span class="sxs-lookup"><span data-stu-id="18a4b-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="18a4b-129">En la siguiente sección trabajará con la cadena de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="18a4b-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="18a4b-130">[Anterior](adding-a-view.md)
> [Siguiente](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="18a4b-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
