---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Agregar un modelo | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model"></a><span data-ttu-id="e68d4-102">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="e68d4-102">Adding a Model</span></span>
====================
<span data-ttu-id="e68d4-103">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e68d4-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="e68d4-104">En esta sección agregará algunas clases para la administración de películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="e68d4-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="e68d4-105">Estas clases será la &quot;modelo&quot; forma parte de la aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e68d4-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="e68d4-106">Deberá usar una tecnología de acceso a datos de .NET Framework conocida como la [Entity Framework](https://docs.microsoft.com/ef/) para definir y trabajar con estas clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="e68d4-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="e68d4-107">El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*.</span><span class="sxs-lookup"><span data-stu-id="e68d4-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="e68d4-108">Código primero le permite crear objetos del modelo mediante la escritura de clases simples.</span><span class="sxs-lookup"><span data-stu-id="e68d4-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="e68d4-109">(Estas son también conocido como las clases, desde &quot;los objetos CLR antiguos sin formato.&quot;) A continuación, puede hacer que la base de datos que se crea sobre la marcha de las clases, lo que permite a un flujo de trabajo de desarrollo muy limpio y rápido.</span><span class="sxs-lookup"><span data-stu-id="e68d4-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="e68d4-110">Si es necesario crear primero la base de datos, puede seguir este tutorial para aprender acerca del desarrollo de aplicaciones MVC y EF.</span><span class="sxs-lookup"><span data-stu-id="e68d4-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="e68d4-111">A continuación, puede seguir Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, que cubre el primer enfoque de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e68d4-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="e68d4-112">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="e68d4-112">Adding Model Classes</span></span>

<span data-ttu-id="e68d4-113">En **el Explorador de soluciones**, haga clic con el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="e68d4-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="e68d4-114">Escriba el *clase* nombre &quot;película&quot;.</span><span class="sxs-lookup"><span data-stu-id="e68d4-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="e68d4-115">Agregue las siguientes propiedades de cinco a la `Movie` clase:</span><span class="sxs-lookup"><span data-stu-id="e68d4-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="e68d4-116">Vamos a usar la `Movie` clase para representar las películas de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="e68d4-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="e68d4-117">Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.</span><span class="sxs-lookup"><span data-stu-id="e68d4-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="e68d4-118">Nota: Para poder usar System.Data.Entity y la clase relacionada, debe instalar la [paquete NuGet de Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="e68d4-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="e68d4-119">Siga el vínculo para obtener más instrucciones.</span><span class="sxs-lookup"><span data-stu-id="e68d4-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="e68d4-120">En el mismo archivo, agregue las siguientes `MovieDBContext` clase:</span><span class="sxs-lookup"><span data-stu-id="e68d4-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="e68d4-121">El `MovieDBContext` clase representa el contexto de base de datos de película de Entity Framework, que controla capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase.</span><span class="sxs-lookup"><span data-stu-id="e68d4-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="e68d4-122">El `MovieDBContext` se deriva de la `DbContext` clase proporcionada por el marco de trabajo de la entidad base.</span><span class="sxs-lookup"><span data-stu-id="e68d4-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="e68d4-123">Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente `using` instrucción en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="e68d4-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="e68d4-124">Puede hacerlo agregando manualmente el uso de la instrucción, o se puede mantener el mouse sobre las líneas onduladas rojas, haga clic en `Show potential fixes` y haga clic en `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="e68d4-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="e68d4-125">Nota: Algunos sin usar `using` instrucciones se han quitado.</span><span class="sxs-lookup"><span data-stu-id="e68d4-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="e68d4-126">Visual Studio mostrará dependencias no usadas en gris.</span><span class="sxs-lookup"><span data-stu-id="e68d4-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="e68d4-127">Puede quitar dependencias no usadas por el mouse sobre las dependencias de gris, haga clic en `Show potential fixes` y haga clic en **quitar instrucciones using no usadas.**</span><span class="sxs-lookup"><span data-stu-id="e68d4-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="e68d4-128">Por último, hemos agregado un modelo (M en MVC).</span><span class="sxs-lookup"><span data-stu-id="e68d4-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="e68d4-129">En la siguiente sección, trabajará con la cadena de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e68d4-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e68d4-130">[Anterior](adding-a-view.md)
> [Siguiente](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="e68d4-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
