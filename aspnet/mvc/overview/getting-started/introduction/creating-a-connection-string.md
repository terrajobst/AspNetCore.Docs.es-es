---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Crear una cadena de conexión y trabajar con SQL Server LocalDB | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: edbd46ef8a03670f0cb7527142babe9bd5846c7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="73105-102">Crear una cadena de conexión y trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="73105-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="73105-103">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="73105-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="73105-104">Crear una cadena de conexión y trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="73105-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="73105-105">El `MovieDBContext` clase creada controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="73105-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="73105-106">Una pregunta que podría preguntar, sin embargo, se muestra cómo especificar qué base de datos se conecta a.</span><span class="sxs-lookup"><span data-stu-id="73105-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="73105-107">Realmente no es necesario que especificar qué base de datos, Entity Framework de forma predeterminada utilizarán [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="73105-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="73105-108">En esta sección vamos a agregar explícitamente una cadena de conexión en el *Web.config* archivo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="73105-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="73105-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="73105-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="73105-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) es una versión ligera de SQL Server Express Database Engine que se inicia a petición y se ejecuta en modo de usuario.</span><span class="sxs-lookup"><span data-stu-id="73105-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="73105-111">LocalDB se ejecuta en un modo especial de ejecución de SQL Server Express que le permite trabajar con bases de datos como *.mdf* archivos.</span><span class="sxs-lookup"><span data-stu-id="73105-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="73105-112">Normalmente, los archivos de base de datos de LocalDB se mantienen en la *aplicación\_datos* carpeta de un proyecto web.</span><span class="sxs-lookup"><span data-stu-id="73105-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="73105-113">SQL Server Express no se recomienda para su uso en aplicaciones web de producción.</span><span class="sxs-lookup"><span data-stu-id="73105-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="73105-114">LocalDB en particular no se recomienda para entornos de producción con una aplicación web porque no está diseñado para trabajar con IIS.</span><span class="sxs-lookup"><span data-stu-id="73105-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="73105-115">Sin embargo, se puede migrar fácilmente una base de datos de LocalDB a SQL Server o SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="73105-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="73105-116">En Visual Studio 2017 LocalDB se instala de forma predeterminada con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73105-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="73105-117">De forma predeterminada, Entity Framework buscará en una cadena de conexión que el mismo nombre que la clase de contexto de objeto (`MovieDBContext` para este proyecto).</span><span class="sxs-lookup"><span data-stu-id="73105-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="73105-118">Para obtener más información, consulte [cadenas de conexión de SQL Server para las aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="73105-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="73105-119">Abrir la raíz de la aplicación *Web.config* archivo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="73105-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="73105-120">(No el *Web.config* un archivo en el *vistas* carpeta.)</span><span class="sxs-lookup"><span data-stu-id="73105-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="73105-121">Buscar el `<connectionStrings>` elemento:</span><span class="sxs-lookup"><span data-stu-id="73105-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="73105-122">Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="73105-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="73105-123">En el ejemplo siguiente se muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:</span><span class="sxs-lookup"><span data-stu-id="73105-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="73105-124">Las dos cadenas de conexión son muy similares.</span><span class="sxs-lookup"><span data-stu-id="73105-124">The two connection strings are very similar.</span></span> <span data-ttu-id="73105-125">La primera cadena de conexión se denomina `DefaultConnection` y se utiliza para la base de datos de pertenencia para controlar quién puede tener acceso a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="73105-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="73105-126">Ha agregado la cadena de conexión especifica una base de datos de LocalDB denominado *Movie.mdf* ubicado en el *aplicación\_datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="73105-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="73105-127">Nos no usar la base de datos de pertenencia en este tutorial, para obtener más información sobre la pertenencia, autenticación y seguridad, vea el tutorial [crear una aplicación de MVC de ASP.NET con la autenticación y la base de datos SQL e implementar al servicio de aplicaciones de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="73105-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="73105-128">El nombre de la cadena de conexión debe coincidir con el nombre de la [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) clase.</span><span class="sxs-lookup"><span data-stu-id="73105-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="73105-129">Realmente no es necesario agregar el `MovieDBContext` cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="73105-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="73105-130">Si no se especifica una cadena de conexión, Entity Framework creará una base de datos de LocalDB en el directorio de los usuarios con el nombre completo de la [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) clase (en este caso `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="73105-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="73105-131">Se puede asignar un nombre la base de datos que desee, siempre y cuando tenga el *. MDF* sufijo.</span><span class="sxs-lookup"><span data-stu-id="73105-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="73105-132">Por ejemplo, se puede nombrar según la base de datos *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="73105-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="73105-133">A continuación, vamos a compilar un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.</span><span class="sxs-lookup"><span data-stu-id="73105-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="73105-134">[Anterior](adding-a-model.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="73105-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
