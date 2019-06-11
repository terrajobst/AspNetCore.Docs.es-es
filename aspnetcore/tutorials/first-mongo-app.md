---
title: Creación de una API Web con ASP.NET Core y MongoDB
author: prkhandelwal
description: En este tutorial se muestra cómo crear una API web de ASP.NET Core con una base de datos NoSQL de MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 06/04/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 6a8c5d75f562b38015101e039a2f5d96a5491595
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692548"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="e8d04-103">Creación de una API Web con ASP.NET Core y MongoDB</span><span class="sxs-lookup"><span data-stu-id="e8d04-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="e8d04-104">Por [Pratik Khandelwal](https://twitter.com/K2Prk) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e8d04-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e8d04-105">En este tutorial se crea una API web que realiza operaciones de creación, lectura, actualización y eliminación (CRUD) en una base de datos NoSQL de [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="e8d04-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="e8d04-106">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="e8d04-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e8d04-107">Configurar MongoDB</span><span class="sxs-lookup"><span data-stu-id="e8d04-107">Configure MongoDB</span></span>
> * <span data-ttu-id="e8d04-108">Crear una base de datos de MongoDB</span><span class="sxs-lookup"><span data-stu-id="e8d04-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="e8d04-109">Definir un esquema y una colección de MongoDB</span><span class="sxs-lookup"><span data-stu-id="e8d04-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="e8d04-110">Realizar operaciones de CRUD de MongoDB desde una API web</span><span class="sxs-lookup"><span data-stu-id="e8d04-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="e8d04-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e8d04-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8d04-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="e8d04-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8d04-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8d04-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="e8d04-114">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="e8d04-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="e8d04-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="e8d04-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="e8d04-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e8d04-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8d04-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8d04-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="e8d04-118">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="e8d04-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="e8d04-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8d04-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="e8d04-120">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8d04-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="e8d04-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e8d04-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8d04-122">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e8d04-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="e8d04-123">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="e8d04-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="e8d04-124">Visual Studio para Mac, versión 7.7 o posterior</span><span class="sxs-lookup"><span data-stu-id="e8d04-124">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="e8d04-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e8d04-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="e8d04-126">Configurar MongoDB</span><span class="sxs-lookup"><span data-stu-id="e8d04-126">Configure MongoDB</span></span>

<span data-ttu-id="e8d04-127">Si usa Windows, MongoDB está instalado en *C:\\Archivos de programa\\MongoDB* de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e8d04-127">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="e8d04-128">Agregue *C:\\Archivos de programa\\MongoDB\\Server\\\<número_versión>\\bin* a la variable de entorno `Path`.</span><span class="sxs-lookup"><span data-stu-id="e8d04-128">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="e8d04-129">Este cambio permite el acceso a MongoDB desde cualquier lugar en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="e8d04-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="e8d04-130">Use el Shell de mongo en los pasos siguientes para crear una base de datos, hacer colecciones y almacenar documentos.</span><span class="sxs-lookup"><span data-stu-id="e8d04-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="e8d04-131">Para obtener más información sobre los comandos de Shell de mongo, consulte [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell) (Trabajo con el shell de Mongo).</span><span class="sxs-lookup"><span data-stu-id="e8d04-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="e8d04-132">Elija un directorio en el equipo de desarrollo para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="e8d04-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="e8d04-133">Por ejemplo, *C:\\BooksData* en Windows.</span><span class="sxs-lookup"><span data-stu-id="e8d04-133">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="e8d04-134">Si no existe el directorio, créelo.</span><span class="sxs-lookup"><span data-stu-id="e8d04-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="e8d04-135">El shell de mongo no crea nuevos directorios.</span><span class="sxs-lookup"><span data-stu-id="e8d04-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="e8d04-136">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="e8d04-136">Open a command shell.</span></span> <span data-ttu-id="e8d04-137">Ejecute el comando siguiente para conectarse a MongoDB en el puerto predeterminado 27017.</span><span class="sxs-lookup"><span data-stu-id="e8d04-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="e8d04-138">No olvide reemplazar `<data_directory_path>` por el directorio que eligió en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="e8d04-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="e8d04-139">Abra otra instancia del shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="e8d04-139">Open another command shell instance.</span></span> <span data-ttu-id="e8d04-140">Conéctese a la base de datos de prueba de forma predeterminada ejecutando el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="e8d04-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="e8d04-141">Ejecute lo siguiente en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="e8d04-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="e8d04-142">Si aún no existe, se crea una base de datos denominada *BookstoreDb*.</span><span class="sxs-lookup"><span data-stu-id="e8d04-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="e8d04-143">Si la base de datos existe, su conexión se abre para las transacciones.</span><span class="sxs-lookup"><span data-stu-id="e8d04-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="e8d04-144">Cree una colección `Books` con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="e8d04-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="e8d04-145">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="e8d04-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="e8d04-146">Defina un esquema para la colección `Books` e inserte dos documentos con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="e8d04-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="e8d04-147">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="e8d04-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="e8d04-148">Vea los documentos en la base de datos mediante el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="e8d04-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="e8d04-149">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="e8d04-149">The following result is displayed:</span></span>

    ```console
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
      "Name" : "Design Patterns",
      "Price" : 54.93,
      "Category" : "Computers",
      "Author" : "Ralph Johnson"
    }
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
      "Name" : "Clean Code",
      "Price" : 43.15,
      "Category" : "Computers",
      "Author" : "Robert C. Martin"
    }
    ```

    <span data-ttu-id="e8d04-150">El esquema agrega una propiedad `_id` generada automáticamente del tipo `ObjectId` para cada documento.</span><span class="sxs-lookup"><span data-stu-id="e8d04-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="e8d04-151">La base de datos está lista.</span><span class="sxs-lookup"><span data-stu-id="e8d04-151">The database is ready.</span></span> <span data-ttu-id="e8d04-152">Puede empezar a crear la API web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8d04-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="e8d04-153">Creación de un proyecto de API web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8d04-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8d04-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8d04-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e8d04-155">Vaya a **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="e8d04-156">Seleccione el tipo de proyecto **Aplicación web de ASP.NET Core** y, luego, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-156">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="e8d04-157">Denomine el proyecto *BooksApi* y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-157">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="e8d04-158">Seleccione el marco de destino **.NET Core** y **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-158">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="e8d04-159">Seleccione la plantilla de proyecto **API** y, luego, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-159">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="e8d04-160">Visite la [galería de NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) para determinar la última versión estable del controlador .NET para MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e8d04-160">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="e8d04-161">En la ventana **Consola del Administrador de paquetes**, desplácese hasta la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e8d04-161">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="e8d04-162">Ejecute el siguiente comando para instalar el controlador .NET para MongoDB:</span><span class="sxs-lookup"><span data-stu-id="e8d04-162">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8d04-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8d04-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="e8d04-164">Ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="e8d04-164">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="e8d04-165">Se genera un nuevo proyecto de API web de ASP.NET Core destinado a .NET Core, que puede abrir en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e8d04-165">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="e8d04-166">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo en el que se le indicará que **faltan los activos necesarios para compilar y depurar en "RazonPagesMovie" y, luego, si quiere agregarlos**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-166">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="e8d04-167">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-167">Select **Yes**.</span></span>
1. <span data-ttu-id="e8d04-168">Visite la [galería de NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) para determinar la última versión estable del controlador .NET para MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e8d04-168">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="e8d04-169">Abra **Terminal integrado** y navegue hasta la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e8d04-169">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="e8d04-170">Ejecute el siguiente comando para instalar el controlador .NET para MongoDB:</span><span class="sxs-lookup"><span data-stu-id="e8d04-170">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8d04-171">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e8d04-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="e8d04-172">Vaya a **Archivo** > **Nueva solución** >  **.NET Core** > **Aplicación**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-172">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="e8d04-173">Seleccione la plantilla de proyecto de C# **API web ASP.NET Core** y, luego, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-173">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="e8d04-174">Seleccione **.NET Core 2.2** en la lista desplegable **Plataforma de destino** y, luego, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-174">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="e8d04-175">Escriba *BooksApi* en **Nombre del proyecto** y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-175">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="e8d04-176">En el panel **Explorador de soluciones**, haga clic con el botón derecho en el nodo **Dependencias** del proyecto y seleccione **Agregar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-176">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="e8d04-177">Escriba *MongoDB.Driver* en el cuadro de búsqueda, seleccione el paquete *MongoDB.Driver* y, luego, **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-177">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="e8d04-178">Seleccione el botón **Aceptar** del cuadro de diálogo **Aceptación de la licencia**.</span><span class="sxs-lookup"><span data-stu-id="e8d04-178">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="e8d04-179">Adición de un modelo de entidad</span><span class="sxs-lookup"><span data-stu-id="e8d04-179">Add an entity model</span></span>

1. <span data-ttu-id="e8d04-180">Agregue un directorio *Modelos* a la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e8d04-180">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="e8d04-181">Agregue una clase `Book` al directorio *Modelos* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="e8d04-181">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

    <span data-ttu-id="e8d04-182">En la clase anterior, se requiere la propiedad `Id`</span><span class="sxs-lookup"><span data-stu-id="e8d04-182">In the preceding class, the `Id` property:</span></span>
    
    * <span data-ttu-id="e8d04-183">para asignar el objeto de Common Language Runtime (CLR) a la colección de MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e8d04-183">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="e8d04-184">Se anota con [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) para designar esta propiedad como clave principal del documento.</span><span class="sxs-lookup"><span data-stu-id="e8d04-184">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="e8d04-185">Se anota con [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) para permitir que el parámetro pase como tipo `string` en lugar de como una estructura [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm).</span><span class="sxs-lookup"><span data-stu-id="e8d04-185">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="e8d04-186">Mongo controla la conversión de `string` a `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="e8d04-186">Mongo handles the conversion from `string` to `ObjectId`.</span></span>
    
    <span data-ttu-id="e8d04-187">Otras propiedades de la clase se anotan con el atributo [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm).</span><span class="sxs-lookup"><span data-stu-id="e8d04-187">Other properties in the class are annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="e8d04-188">El valor del atributo representa el nombre de propiedad en la colección de MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e8d04-188">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="e8d04-189">Adición de un modelo configuración</span><span class="sxs-lookup"><span data-stu-id="e8d04-189">Add a configuration model</span></span>

1. <span data-ttu-id="e8d04-190">Agregue los siguientes valores de configuración de base de datos a *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="e8d04-190">Add the following database configuration values to *appsettings.json*:</span></span>

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="e8d04-191">Agregue un archivo *BookstoreDatabaseSettings.cs* al directorio *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="e8d04-191">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    <span data-ttu-id="e8d04-192">La clase anterior `BookstoreDatabaseSettings` se utiliza para almacenar los valores de propiedad `BookstoreDatabaseSettings` del archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e8d04-192">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="e8d04-193">Los nombres de las propiedades de JSON y C# son iguales para facilitar el proceso de asignación.</span><span class="sxs-lookup"><span data-stu-id="e8d04-193">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="e8d04-194">Agregue el código siguiente a `Startup.ConfigureServices` antes de llamar a `AddMvc`:</span><span class="sxs-lookup"><span data-stu-id="e8d04-194">Add the following code to `Startup.ConfigureServices`, before the call to `AddMvc`:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureDatabaseSettings)]

    <span data-ttu-id="e8d04-195">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="e8d04-195">In the preceding code:</span></span>

    * <span data-ttu-id="e8d04-196">La instancia de configuración a la que la sección `BookstoreDatabaseSettings` del archivo *appsettings.json* enlaza está registrada en el contenedor de inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="e8d04-196">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="e8d04-197">Por ejemplo, una propiedad `ConnectionString` del objeto `BookstoreDatabaseSettings` se rellena con la propiedad `BookstoreDatabaseSettings:ConnectionString` en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e8d04-197">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="e8d04-198">La interfaz `IBookstoreDatabaseSettings` se registra en la inserción de dependencias con una [duración de servicio](xref:fundamentals/dependency-injection#service-lifetimes) de tipo singleton.</span><span class="sxs-lookup"><span data-stu-id="e8d04-198">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="e8d04-199">Cuando se inserta, la instancia de la interfaz se resuelve en un objeto `BookstoreDatabaseSettings`.</span><span class="sxs-lookup"><span data-stu-id="e8d04-199">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="e8d04-200">Agregue el código siguiente en la parte superior del archivo *Startup.cs* para resolver las referencias a `BookstoreDatabaseSettings` y `IBookstoreDatabaseSettings`:</span><span class="sxs-lookup"><span data-stu-id="e8d04-200">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="e8d04-201">Adición de un servicio de operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="e8d04-201">Add a CRUD operations service</span></span>

1. <span data-ttu-id="e8d04-202">Agregue un directorio *Servicios* a la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e8d04-202">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="e8d04-203">Agregue una clase `BookService` al directorio *Servicios* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="e8d04-203">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    <span data-ttu-id="e8d04-204">En el código anterior, se recuperó una instancia de `IBookstoreDatabaseSettings` de la inserción de dependencias mediante la inserción de un constructor.</span><span class="sxs-lookup"><span data-stu-id="e8d04-204">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="e8d04-205">Esta técnica proporciona acceso a los valores de configuración de *appsettings.json* que se agregaron en la sección [Adición de un modelo de configuración](#add-a-configuration-model).</span><span class="sxs-lookup"><span data-stu-id="e8d04-205">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="e8d04-206">En `Startup.ConfigureServices`, registre la clase `BookService` con inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="e8d04-206">In `Startup.ConfigureServices`, register the `BookService` class with DI:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=9)]

    <span data-ttu-id="e8d04-207">En el código anterior, la clase `BookService` se registra con inserción de dependencias para admitir la inserción del constructor en las clases de consumo.</span><span class="sxs-lookup"><span data-stu-id="e8d04-207">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="e8d04-208">La duración de servicio de tipo singleton es la más adecuada porque `BookService` toma una dependencia directa sobre `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="e8d04-208">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="e8d04-209">Según las [instrucciones oficiales de reutilización de cliente Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` debe registrarse en la inserción de dependencias con una duración de servicio de tipo singleton.</span><span class="sxs-lookup"><span data-stu-id="e8d04-209">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="e8d04-210">Agregue el código siguiente en la parte superior del archivo *Startup.cs* para resolver la referencia a `BookService`:</span><span class="sxs-lookup"><span data-stu-id="e8d04-210">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="e8d04-211">La clase`BookService` usa los miembros `MongoDB.Driver` siguientes para realizar operaciones CRUD en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="e8d04-211">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="e8d04-212">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm): lee la instancia del servidor para realizar operaciones de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e8d04-212">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="e8d04-213">Se proporciona la cadena de conexión de MongoDB al constructor de esta clase:</span><span class="sxs-lookup"><span data-stu-id="e8d04-213">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="e8d04-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm): representa la base de datos de Mongo para realizar operaciones.</span><span class="sxs-lookup"><span data-stu-id="e8d04-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="e8d04-215">Este tutorial usa el método genérico [GetCollection<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) en la interfaz para tener acceso a los datos de una colección específica.</span><span class="sxs-lookup"><span data-stu-id="e8d04-215">This tutorial uses the generic [GetCollection<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="e8d04-216">Realice las operaciones CRUD en la colección después de llamar a este método.</span><span class="sxs-lookup"><span data-stu-id="e8d04-216">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="e8d04-217">En la llamada al método `GetCollection<TDocument>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="e8d04-217">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="e8d04-218">`collection` representa el nombre de la colección.</span><span class="sxs-lookup"><span data-stu-id="e8d04-218">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="e8d04-219">`TDocument` representa el tipo de objeto CLR almacenado en la colección.</span><span class="sxs-lookup"><span data-stu-id="e8d04-219">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="e8d04-220">`GetCollection<TDocument>(collection)` devuelve un objeto [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) que representa la colección.</span><span class="sxs-lookup"><span data-stu-id="e8d04-220">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="e8d04-221">En este tutorial, se invocan los métodos siguientes en la colección:</span><span class="sxs-lookup"><span data-stu-id="e8d04-221">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="e8d04-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm): elimina un único documento que cumpla los criterios de búsqueda proporcionados.</span><span class="sxs-lookup"><span data-stu-id="e8d04-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="e8d04-223">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm): devuelve todos los documentos de la colección que cumplen los criterios de búsqueda indicados.</span><span class="sxs-lookup"><span data-stu-id="e8d04-223">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="e8d04-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm): inserta el objeto proporcionado como un nuevo documento en la colección.</span><span class="sxs-lookup"><span data-stu-id="e8d04-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="e8d04-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm): reemplaza un único documento que cumpla los criterios de búsqueda indicados por el objeto proporcionado.</span><span class="sxs-lookup"><span data-stu-id="e8d04-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="e8d04-226">Incorporación de un controlador</span><span class="sxs-lookup"><span data-stu-id="e8d04-226">Add a controller</span></span>

<span data-ttu-id="e8d04-227">Agregue una clase `BooksController` al directorio *Controladores* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="e8d04-227">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

<span data-ttu-id="e8d04-228">El controlador de API web anterior:</span><span class="sxs-lookup"><span data-stu-id="e8d04-228">The preceding web API controller:</span></span>

* <span data-ttu-id="e8d04-229">Usa la clase `BookService` para realizar operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="e8d04-229">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="e8d04-230">Contiene métodos de acción para admitir las solicitudes GET, POST, PUT y DELETE de HTTP.</span><span class="sxs-lookup"><span data-stu-id="e8d04-230">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="e8d04-231">Llama a <xref:System.Web.Http.ApiController.CreatedAtRoute*> en el método de acción `Create` para devolver una respuesta [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e8d04-231">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="e8d04-232">El código de estado 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e8d04-232">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="e8d04-233">`CreatedAtRoute` también agrega un encabezado `Location` a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="e8d04-233">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="e8d04-234">El encabezado `Location` especifica el identificador URI del libro recién creado.</span><span class="sxs-lookup"><span data-stu-id="e8d04-234">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="e8d04-235">Prueba de la API web</span><span class="sxs-lookup"><span data-stu-id="e8d04-235">Test the web API</span></span>

1. <span data-ttu-id="e8d04-236">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e8d04-236">Build and run the app.</span></span>

1. <span data-ttu-id="e8d04-237">Vaya a `http://localhost:<port>/api/books` para probar el método de acción `Get` sin parámetros del controlador.</span><span class="sxs-lookup"><span data-stu-id="e8d04-237">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="e8d04-238">Se muestra la siguiente respuesta JSON:</span><span class="sxs-lookup"><span data-stu-id="e8d04-238">The following JSON response is displayed:</span></span>

    ```json
    [
      {
        "id":"5bfd996f7b8e48dc15ff215d",
        "bookName":"Design Patterns",
        "price":54.93,
        "category":"Computers",
        "author":"Ralph Johnson"
      },
      {
        "id":"5bfd996f7b8e48dc15ff215e",
        "bookName":"Clean Code",
        "price":43.15,
        "category":"Computers",
        "author":"Robert C. Martin"
      }
    ]
    ```

1. <span data-ttu-id="e8d04-239">Vaya a `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` para probar el método de acción `Get` sobrecargado del controlador.</span><span class="sxs-lookup"><span data-stu-id="e8d04-239">Navigate to `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="e8d04-240">Se muestra la siguiente respuesta JSON:</span><span class="sxs-lookup"><span data-stu-id="e8d04-240">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"5bfd996f7b8e48dc15ff215e",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="e8d04-241">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="e8d04-241">Next steps</span></span>

<span data-ttu-id="e8d04-242">Para obtener más información sobre la creación de las API web de ASP.NET Core, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="e8d04-242">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="e8d04-243">Versión de YouTube de este artículo</span><span class="sxs-lookup"><span data-stu-id="e8d04-243">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
