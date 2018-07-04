---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Adición de un modelo | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: ea8700020c1cc4b8ca88f657bd46b82e61d6f40f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388576"
---
<a name="adding-a-model"></a><span data-ttu-id="5f9f6-104">Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="5f9f6-104">Adding a Model</span></span>
====================
<span data-ttu-id="5f9f6-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="5f9f6-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="5f9f6-106">Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="5f9f6-107">Es más seguro y mucho más fácil de seguir y se muestran las características más.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="5f9f6-108">En esta sección agregará algunas clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="5f9f6-109">Estas clases serán el &quot;modelo&quot; forma parte de la aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="5f9f6-110">Deberá usar una tecnología de acceso a datos de .NET Framework conocida como la [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) para definir y trabajar con estas clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="5f9f6-111">El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="5f9f6-112">Código primero permite crear objetos de modelo mediante la escritura de clases simple.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="5f9f6-113">(También conocido como son las clases POCO, de &quot;los objetos CLR antiguos sin formato.&quot;) A continuación, puede tener la base de datos que se crean sobre la marcha de las clases, lo que permite un flujo de trabajo de desarrollo muy limpio y rápido.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="5f9f6-114">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="5f9f6-114">Adding Model Classes</span></span>

<span data-ttu-id="5f9f6-115">En **el Explorador de soluciones**, haga clic en el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="5f9f6-116">Escriba el *clase* nombre &quot;película&quot;.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="5f9f6-117">Agregue las siguientes cinco propiedades para el `Movie` clase:</span><span class="sxs-lookup"><span data-stu-id="5f9f6-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="5f9f6-118">Vamos a usar la `Movie` clase para representar las películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="5f9f6-119">Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="5f9f6-120">En el mismo archivo, agregue la siguiente `MovieDBContext` clase:</span><span class="sxs-lookup"><span data-stu-id="5f9f6-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="5f9f6-121">El `MovieDBContext` clase representa el contexto de base de datos de películas de Entity Framework, que se encarga de capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="5f9f6-122">El `MovieDBContext` deriva el `DbContext` proporcionado por Entity Framework de clase base.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="5f9f6-123">Para poder hacer referencia a `DbContext` y `DbSet`, deberá agregar lo siguiente `using` instrucción en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="5f9f6-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="5f9f6-124">La completa *Movie.cs* archivo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="5f9f6-125">(Varias instrucciones using que no son necesitan se han quitado.)</span><span class="sxs-lookup"><span data-stu-id="5f9f6-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="5f9f6-126">Creación de una cadena de conexión y trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="5f9f6-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="5f9f6-127">El `MovieDBContext` clase creó controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="5f9f6-128">Una pregunta que podría preguntar, sin embargo, es cómo especificar que se conectará a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="5f9f6-129">Hágalo mediante la adición de información de conexión en el *Web.config* archivo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="5f9f6-130">Abra la raíz de la aplicación *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="5f9f6-131">(No el *Web.config* de archivos en el *vistas* carpeta.) Abra el *Web.config* archivo destacado en rojo.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="5f9f6-132">Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="5f9f6-133">El ejemplo siguiente muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:</span><span class="sxs-lookup"><span data-stu-id="5f9f6-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="5f9f6-134">Esta pequeña cantidad de código y XML es todo lo que necesita para escribir con el fin de representar y almacenar los datos de la película en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="5f9f6-135">A continuación, creará un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.</span><span class="sxs-lookup"><span data-stu-id="5f9f6-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5f9f6-136">[Anterior](adding-a-view.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="5f9f6-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
