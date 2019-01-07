---
title: Compilación de API web con ASP.NET Core y MongoDB
author: prkhandelwal
description: En este tutorial se muestra cómo crear una API web de ASP.NET Core con una base de datos NoSQL de MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 11/29/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: bd9a36c5eb06542c820e71e937b8da10f735a0f8
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577843"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="18e39-103">Creación de una API Web con ASP.NET Core y MongoDB</span><span class="sxs-lookup"><span data-stu-id="18e39-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="18e39-104">Por [Pratik Khandelwal](https://twitter.com/K2Prk) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="18e39-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="18e39-105">En este tutorial se crea una API web que realiza operaciones de creación, lectura, actualización y eliminación (CRUD) en una base de datos NoSQL de [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="18e39-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="18e39-106">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="18e39-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="18e39-107">Configurar MongoDB</span><span class="sxs-lookup"><span data-stu-id="18e39-107">Configure MongoDB</span></span>
> * <span data-ttu-id="18e39-108">Crear una base de datos de MongoDB</span><span class="sxs-lookup"><span data-stu-id="18e39-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="18e39-109">Definir un esquema y una colección de MongoDB</span><span class="sxs-lookup"><span data-stu-id="18e39-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="18e39-110">Realizar operaciones de CRUD de MongoDB desde una API web</span><span class="sxs-lookup"><span data-stu-id="18e39-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="18e39-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="18e39-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18e39-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="18e39-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="18e39-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="18e39-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="18e39-114">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="18e39-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="18e39-115">[Visual Studio 2017 versión 15.9 o posterior](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="18e39-115">[Visual Studio 2017 version 15.9 or later](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="18e39-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="18e39-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="18e39-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="18e39-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="18e39-118">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="18e39-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="18e39-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="18e39-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="18e39-120">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="18e39-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="18e39-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="18e39-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="18e39-122">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="18e39-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="18e39-123">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="18e39-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="18e39-124">Visual Studio para Mac, versión 7.7 o posterior</span><span class="sxs-lookup"><span data-stu-id="18e39-124">Visual Studio for Mac version 7.7 or later</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="18e39-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="18e39-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="18e39-126">Configurar MongoDB</span><span class="sxs-lookup"><span data-stu-id="18e39-126">Configure MongoDB</span></span>

<span data-ttu-id="18e39-127">Si usa Windows, MongoDB está instalado en *C:\Archivos de programa\MongoDB* de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="18e39-127">If using Windows, MongoDB is installed at *C:\Program Files\MongoDB* by default.</span></span> <span data-ttu-id="18e39-128">Agregue *C:\Archivos de programa\MongoDB\Server\<número_versión>\bin* a la variable de entorno `Path`.</span><span class="sxs-lookup"><span data-stu-id="18e39-128">Add *C:\Program Files\MongoDB\Server\<version_number>\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="18e39-129">Este cambio permite el acceso a MongoDB desde cualquier lugar en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="18e39-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="18e39-130">Use el Shell de mongo en los pasos siguientes para crear una base de datos, hacer colecciones y almacenar documentos.</span><span class="sxs-lookup"><span data-stu-id="18e39-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="18e39-131">Para obtener más información sobre los comandos de Shell de mongo, consulte [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell) (Trabajo con el shell de Mongo).</span><span class="sxs-lookup"><span data-stu-id="18e39-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="18e39-132">Elija un directorio en el equipo de desarrollo para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="18e39-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="18e39-133">Por ejemplo, *C:\BooksData* en Windows.</span><span class="sxs-lookup"><span data-stu-id="18e39-133">For example, *C:\BooksData* on Windows.</span></span> <span data-ttu-id="18e39-134">Si no existe el directorio, créelo.</span><span class="sxs-lookup"><span data-stu-id="18e39-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="18e39-135">El shell de mongo no crea nuevos directorios.</span><span class="sxs-lookup"><span data-stu-id="18e39-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="18e39-136">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="18e39-136">Open a command shell.</span></span> <span data-ttu-id="18e39-137">Ejecute el comando siguiente para conectarse a MongoDB en el puerto predeterminado 27017.</span><span class="sxs-lookup"><span data-stu-id="18e39-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="18e39-138">No olvide reemplazar `<data_directory_path>` por el directorio que eligió en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="18e39-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="18e39-139">Abra otra instancia del shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="18e39-139">Open another command shell instance.</span></span> <span data-ttu-id="18e39-140">Conéctese a la base de datos de prueba de forma predeterminada ejecutando el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="18e39-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="18e39-141">Ejecute lo siguiente en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="18e39-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="18e39-142">Si aún no existe, se crea una base de datos denominada *BookstoreDb*.</span><span class="sxs-lookup"><span data-stu-id="18e39-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="18e39-143">Si la base de datos existe, su conexión se abre para las transacciones.</span><span class="sxs-lookup"><span data-stu-id="18e39-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="18e39-144">Cree una colección `Books` con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="18e39-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="18e39-145">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="18e39-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="18e39-146">Defina un esquema para la colección `Books` e inserte dos documentos con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="18e39-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="18e39-147">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="18e39-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="18e39-148">Vea los documentos en la base de datos mediante el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="18e39-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="18e39-149">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="18e39-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="18e39-150">El esquema agrega una propiedad `_id` generada automáticamente del tipo `ObjectId` para cada documento.</span><span class="sxs-lookup"><span data-stu-id="18e39-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="18e39-151">La base de datos está lista.</span><span class="sxs-lookup"><span data-stu-id="18e39-151">The database is ready.</span></span> <span data-ttu-id="18e39-152">Puede empezar a crear la API web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="18e39-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="18e39-153">Creación de un proyecto de API web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="18e39-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="18e39-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="18e39-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="18e39-155">Vaya a **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="18e39-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="18e39-156">Seleccione **Aplicación web de ASP.NET Core**, ponga al proyecto el nombre *BooksApi*y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="18e39-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="18e39-157">Seleccione el marco de destino **.NET Core** y **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="18e39-157">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="18e39-158">Seleccione la plantilla de proyecto **API** y haga clic en **Aceptar**:</span><span class="sxs-lookup"><span data-stu-id="18e39-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="18e39-159">En la ventana **Consola del Administrador de paquetes**, desplácese hasta la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="18e39-159">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="18e39-160">Ejecute el siguiente comando para instalar el controlador .NET para MongoDB:</span><span class="sxs-lookup"><span data-stu-id="18e39-160">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="18e39-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="18e39-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="18e39-162">Ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="18e39-162">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="18e39-163">Se genera un nuevo proyecto de API web de ASP.NET Core destinado a .NET Core, que puede abrir en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="18e39-163">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="18e39-164">Haga clic en **Sí** cuando aparezca la notificación *Required assets to build and debug are missing from 'BooksApi'. Add them?* (Faltan los activos necesarios para compilar y depurar en 'BooksApi'. ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="18e39-164">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="18e39-165">Abra **Terminal integrado** y navegue hasta la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="18e39-165">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="18e39-166">Ejecute el siguiente comando para instalar el controlador .NET para MongoDB:</span><span class="sxs-lookup"><span data-stu-id="18e39-166">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v 2.7.2
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="18e39-167">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="18e39-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="18e39-168">Vaya a **Archivo** > **Nueva solución** > **.NET Core** > **Aplicación**.</span><span class="sxs-lookup"><span data-stu-id="18e39-168">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="18e39-169">Seleccione la plantilla de proyecto de C# **API web ASP.NET Core** y haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="18e39-169">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="18e39-170">Seleccione **.NET Core 2.2** en la lista desplegable **Plataforma de destino** y haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="18e39-170">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="18e39-171">Escriba *BooksApi* en **Nombre del proyecto** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="18e39-171">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="18e39-172">En el panel **Explorador de soluciones**, haga clic con el botón derecho en el nodo **Dependencias** del proyecto y seleccione **Agregar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="18e39-172">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="18e39-173">Escriba *MongoDB.Driver* en el cuadro de búsqueda, seleccione el paquete *MongoDB.Driver* y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="18e39-173">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="18e39-174">Haga clic en el botón **Aceptar** del cuadro de diálogo **Aceptación de la licencia**.</span><span class="sxs-lookup"><span data-stu-id="18e39-174">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="18e39-175">Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="18e39-175">Add a model</span></span>

1. <span data-ttu-id="18e39-176">Agregue un directorio *Modelos* a la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="18e39-176">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="18e39-177">Agregue una clase `Book` al directorio *Modelos* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="18e39-177">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="18e39-178">En la clase anterior, se requiere la propiedad `Id` para asignar el objeto de Common Language Runtime (CLR) a la colección de MongoDB.</span><span class="sxs-lookup"><span data-stu-id="18e39-178">In the preceding class, the `Id` property is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span> <span data-ttu-id="18e39-179">Otras propiedades de la clase se decoran con el atributo `[BsonElement]`.</span><span class="sxs-lookup"><span data-stu-id="18e39-179">Other properties in the class are decorated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="18e39-180">El valor del atributo representa el nombre de propiedad en la colección de MongoDB.</span><span class="sxs-lookup"><span data-stu-id="18e39-180">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="18e39-181">Adición de una clase de las operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="18e39-181">Add a CRUD operations class</span></span>

1. <span data-ttu-id="18e39-182">Agregue un directorio *Servicios* a la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="18e39-182">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="18e39-183">Agregue una clase `BookService` al directorio *Servicios* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="18e39-183">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="18e39-184">Agregue una cadena de conexión MongoDB a *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="18e39-184">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="18e39-185">Se tiene acceso a la propiedad `BookstoreDb` anterior en el constructor de clase `BookService`.</span><span class="sxs-lookup"><span data-stu-id="18e39-185">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="18e39-186">En `Startup.ConfigureServices`, registre la clase `BookService` con el sistema de inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="18e39-186">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="18e39-187">El registro del servicio anterior es necesario para admitir la inserción del constructor en las clases de consumo.</span><span class="sxs-lookup"><span data-stu-id="18e39-187">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="18e39-188">La clase`BookService` usa los miembros `MongoDB.Driver` siguientes para realizar operaciones CRUD en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="18e39-188">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="18e39-189">`MongoClient` &ndash; Lee la instancia del servidor para realizar operaciones de base de datos.</span><span class="sxs-lookup"><span data-stu-id="18e39-189">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="18e39-190">Se proporciona la cadena de conexión de MongoDB al constructor de esta clase:</span><span class="sxs-lookup"><span data-stu-id="18e39-190">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="18e39-191">`IMongoDatabase` &ndash; Representa la base de datos de Mongo para realizar operaciones.</span><span class="sxs-lookup"><span data-stu-id="18e39-191">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="18e39-192">Este tutorial usa el método genérico `GetCollection<T>(collection)` de la interfaz para tener acceso a los datos de una colección específica.</span><span class="sxs-lookup"><span data-stu-id="18e39-192">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="18e39-193">Las operaciones CRUD pueden realizarse en la colección después de llamar a este método.</span><span class="sxs-lookup"><span data-stu-id="18e39-193">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="18e39-194">En la llamada al método `GetCollection<T>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="18e39-194">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="18e39-195">`collection` representa el nombre de la colección.</span><span class="sxs-lookup"><span data-stu-id="18e39-195">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="18e39-196">`T` representa el tipo de objeto CLR almacenado en la colección.</span><span class="sxs-lookup"><span data-stu-id="18e39-196">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="18e39-197">`GetCollection<T>(collection)` devuelve un objeto `MongoCollection` que representa la colección.</span><span class="sxs-lookup"><span data-stu-id="18e39-197">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="18e39-198">En este tutorial, se invocan los métodos siguientes en la colección:</span><span class="sxs-lookup"><span data-stu-id="18e39-198">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="18e39-199">`Find<T>` &ndash; Devuelve todos los documentos de la colección que cumplen los criterios de búsqueda indicados.</span><span class="sxs-lookup"><span data-stu-id="18e39-199">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="18e39-200">`InsertOne` &ndash; Inserta el objeto proporcionado como un nuevo documento en la colección.</span><span class="sxs-lookup"><span data-stu-id="18e39-200">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="18e39-201">`ReplaceOne` &ndash; Reemplaza un único documento que cumpla los criterios de búsqueda indicados por el objeto proporcionado.</span><span class="sxs-lookup"><span data-stu-id="18e39-201">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="18e39-202">`DeleteOne` &ndash; Elimina un único documento que cumpla los criterios de búsqueda proporcionados.</span><span class="sxs-lookup"><span data-stu-id="18e39-202">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="18e39-203">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="18e39-203">Add a controller</span></span>

1. <span data-ttu-id="18e39-204">Agregue una clase `BooksController` al directorio *Controladores* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="18e39-204">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="18e39-205">El controlador de API web anterior:</span><span class="sxs-lookup"><span data-stu-id="18e39-205">The preceding web API controller:</span></span>

    * <span data-ttu-id="18e39-206">Usa la clase `BookService` para realizar operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="18e39-206">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="18e39-207">Contiene métodos de acción para admitir las solicitudes GET, POST, PUT y DELETE de HTTP.</span><span class="sxs-lookup"><span data-stu-id="18e39-207">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="18e39-208">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="18e39-208">Build and run the app.</span></span>
1. <span data-ttu-id="18e39-209">Vaya a `http://localhost:<port>/api/books` en el explorador.</span><span class="sxs-lookup"><span data-stu-id="18e39-209">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="18e39-210">Se muestra la siguiente respuesta JSON:</span><span class="sxs-lookup"><span data-stu-id="18e39-210">The following JSON response is displayed:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="18e39-211">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="18e39-211">Next steps</span></span>

<span data-ttu-id="18e39-212">Para obtener más información sobre la creación de las API web de ASP.NET Core, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="18e39-212">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
