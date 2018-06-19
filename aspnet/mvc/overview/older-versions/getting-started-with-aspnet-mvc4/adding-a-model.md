---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Agregar un modelo | Documentos de Microsoft
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 562a06e22aad62b6982aca3316a2dfe18a6eba2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871966"
---
<a name="adding-a-model"></a><span data-ttu-id="adc4d-104">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="adc4d-104">Adding a Model</span></span>
====================
<span data-ttu-id="adc4d-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="adc4d-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="adc4d-106">Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="adc4d-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="adc4d-107">Es más seguro y mucho más fácil de seguir y se muestra más características.</span><span class="sxs-lookup"><span data-stu-id="adc4d-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="adc4d-108">En esta sección agregará algunas clases para la administración de películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="adc4d-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="adc4d-109">Estas clases será la &quot;modelo&quot; forma parte de la aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="adc4d-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="adc4d-110">Deberá usar una tecnología de acceso a datos de .NET Framework conocida como la [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) para definir y trabajar con estas clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="adc4d-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="adc4d-111">El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*.</span><span class="sxs-lookup"><span data-stu-id="adc4d-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="adc4d-112">Código primero le permite crear objetos del modelo mediante la escritura de clases simples.</span><span class="sxs-lookup"><span data-stu-id="adc4d-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="adc4d-113">(Estas son también conocido como las clases, desde &quot;los objetos CLR antiguos sin formato.&quot;) A continuación, puede hacer que la base de datos que se crea sobre la marcha de las clases, lo que permite a un flujo de trabajo de desarrollo muy limpio y rápido.</span><span class="sxs-lookup"><span data-stu-id="adc4d-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="adc4d-114">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="adc4d-114">Adding Model Classes</span></span>

<span data-ttu-id="adc4d-115">En **el Explorador de soluciones**, haga clic con el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="adc4d-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="adc4d-116">Escriba el *clase* nombre &quot;película&quot;.</span><span class="sxs-lookup"><span data-stu-id="adc4d-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="adc4d-117">Agregue las siguientes propiedades de cinco a la `Movie` clase:</span><span class="sxs-lookup"><span data-stu-id="adc4d-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="adc4d-118">Vamos a usar la `Movie` clase para representar las películas de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="adc4d-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="adc4d-119">Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.</span><span class="sxs-lookup"><span data-stu-id="adc4d-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="adc4d-120">En el mismo archivo, agregue las siguientes `MovieDBContext` clase:</span><span class="sxs-lookup"><span data-stu-id="adc4d-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="adc4d-121">El `MovieDBContext` clase representa el contexto de base de datos de película de Entity Framework, que controla capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase.</span><span class="sxs-lookup"><span data-stu-id="adc4d-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="adc4d-122">El `MovieDBContext` se deriva de la `DbContext` clase proporcionada por el marco de trabajo de la entidad base.</span><span class="sxs-lookup"><span data-stu-id="adc4d-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="adc4d-123">Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente `using` instrucción en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="adc4d-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="adc4d-124">La sección completa *Movie.cs* archivo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="adc4d-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="adc4d-125">(Varias instrucciones using que no son necesarios se han quitado.)</span><span class="sxs-lookup"><span data-stu-id="adc4d-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="adc4d-126">Crear una cadena de conexión y trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="adc4d-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="adc4d-127">El `MovieDBContext` clase creada controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="adc4d-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="adc4d-128">Una pregunta que podría preguntar, sin embargo, se muestra cómo especificar qué base de datos se conecta a.</span><span class="sxs-lookup"><span data-stu-id="adc4d-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="adc4d-129">Llevará a cabo mediante la adición de información de conexión en el *Web.config* archivo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adc4d-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="adc4d-130">Abrir la raíz de la aplicación *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="adc4d-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="adc4d-131">(No el *Web.config* un archivo en el *vistas* carpeta.) Abra la *Web.config* archivo destacado en rojo.</span><span class="sxs-lookup"><span data-stu-id="adc4d-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="adc4d-132">Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="adc4d-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="adc4d-133">En el ejemplo siguiente se muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:</span><span class="sxs-lookup"><span data-stu-id="adc4d-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="adc4d-134">Esta cantidad pequeña de código y XML es todo lo que necesario para escribir con el fin de representar y almacenar los datos de la película en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="adc4d-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="adc4d-135">A continuación, vamos a compilar un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.</span><span class="sxs-lookup"><span data-stu-id="adc4d-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="adc4d-136">[Anterior](adding-a-view.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="adc4d-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
