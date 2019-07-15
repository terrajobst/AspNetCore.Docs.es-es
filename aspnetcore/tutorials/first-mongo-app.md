---
title: Creación de una API Web con ASP.NET Core y MongoDB
author: prkhandelwal
description: En este tutorial se muestra cómo crear una API web de ASP.NET Core con una base de datos NoSQL de MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: c64f6e69a19e294a18cc72c860af0a03ef70d444
ms.sourcegitcommit: 357a7120632b20465801c093e4e5bd4a315496a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649190"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="19417-103">Creación de una API Web con ASP.NET Core y MongoDB</span><span class="sxs-lookup"><span data-stu-id="19417-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="19417-104">Por [Pratik Khandelwal](https://twitter.com/K2Prk) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="19417-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="19417-105">En este tutorial se crea una API web que realiza operaciones de creación, lectura, actualización y eliminación (CRUD) en una base de datos NoSQL de [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="19417-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="19417-106">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="19417-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="19417-107">Configurar MongoDB</span><span class="sxs-lookup"><span data-stu-id="19417-107">Configure MongoDB</span></span>
> * <span data-ttu-id="19417-108">Crear una base de datos de MongoDB</span><span class="sxs-lookup"><span data-stu-id="19417-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="19417-109">Definir un esquema y una colección de MongoDB</span><span class="sxs-lookup"><span data-stu-id="19417-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="19417-110">Realizar operaciones de CRUD de MongoDB desde una API web</span><span class="sxs-lookup"><span data-stu-id="19417-110">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="19417-111">Personalizar la serialización de JSON</span><span class="sxs-lookup"><span data-stu-id="19417-111">Customize JSON serialization</span></span>

<span data-ttu-id="19417-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19417-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19417-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="19417-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="19417-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="19417-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="19417-115">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="19417-115">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="19417-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="19417-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="19417-117">MongoDB</span><span class="sxs-lookup"><span data-stu-id="19417-117">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="19417-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19417-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="19417-119">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="19417-119">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="19417-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19417-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="19417-121">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19417-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="19417-122">MongoDB</span><span class="sxs-lookup"><span data-stu-id="19417-122">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="19417-123">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="19417-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="19417-124">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="19417-124">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="19417-125">Visual Studio para Mac, versión 7.7 o posterior</span><span class="sxs-lookup"><span data-stu-id="19417-125">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="19417-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="19417-126">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="19417-127">Configurar MongoDB</span><span class="sxs-lookup"><span data-stu-id="19417-127">Configure MongoDB</span></span>

<span data-ttu-id="19417-128">Si usa Windows, MongoDB está instalado en *C:\\Archivos de programa\\MongoDB* de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="19417-128">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="19417-129">Agregue *C:\\Archivos de programa\\MongoDB\\Server\\\<número_versión>\\bin* a la variable de entorno `Path`.</span><span class="sxs-lookup"><span data-stu-id="19417-129">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="19417-130">Este cambio permite el acceso a MongoDB desde cualquier lugar en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="19417-130">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="19417-131">Use el Shell de mongo en los pasos siguientes para crear una base de datos, hacer colecciones y almacenar documentos.</span><span class="sxs-lookup"><span data-stu-id="19417-131">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="19417-132">Para obtener más información sobre los comandos de Shell de mongo, consulte [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell) (Trabajo con el shell de Mongo).</span><span class="sxs-lookup"><span data-stu-id="19417-132">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="19417-133">Elija un directorio en el equipo de desarrollo para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="19417-133">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="19417-134">Por ejemplo, *C:\\BooksData* en Windows.</span><span class="sxs-lookup"><span data-stu-id="19417-134">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="19417-135">Si no existe el directorio, créelo.</span><span class="sxs-lookup"><span data-stu-id="19417-135">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="19417-136">El shell de mongo no crea nuevos directorios.</span><span class="sxs-lookup"><span data-stu-id="19417-136">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="19417-137">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="19417-137">Open a command shell.</span></span> <span data-ttu-id="19417-138">Ejecute el comando siguiente para conectarse a MongoDB en el puerto predeterminado 27017.</span><span class="sxs-lookup"><span data-stu-id="19417-138">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="19417-139">No olvide reemplazar `<data_directory_path>` por el directorio que eligió en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="19417-139">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="19417-140">Abra otra instancia del shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="19417-140">Open another command shell instance.</span></span> <span data-ttu-id="19417-141">Conéctese a la base de datos de prueba de forma predeterminada ejecutando el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="19417-141">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="19417-142">Ejecute lo siguiente en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="19417-142">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="19417-143">Si aún no existe, se crea una base de datos denominada *BookstoreDb*.</span><span class="sxs-lookup"><span data-stu-id="19417-143">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="19417-144">Si la base de datos existe, su conexión se abre para las transacciones.</span><span class="sxs-lookup"><span data-stu-id="19417-144">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="19417-145">Cree una colección `Books` con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="19417-145">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="19417-146">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="19417-146">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="19417-147">Defina un esquema para la colección `Books` e inserte dos documentos con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="19417-147">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="19417-148">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="19417-148">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```
  
  > [!NOTE]
  > <span data-ttu-id="19417-149">Los identificadores que se muestran en este artículo no coinciden con los que se mostrarán cuando ejecute este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="19417-149">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="19417-150">Vea los documentos en la base de datos mediante el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="19417-150">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="19417-151">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="19417-151">The following result is displayed:</span></span>

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

    <span data-ttu-id="19417-152">El esquema agrega una propiedad `_id` generada automáticamente del tipo `ObjectId` para cada documento.</span><span class="sxs-lookup"><span data-stu-id="19417-152">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="19417-153">La base de datos está lista.</span><span class="sxs-lookup"><span data-stu-id="19417-153">The database is ready.</span></span> <span data-ttu-id="19417-154">Puede empezar a crear la API web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19417-154">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="19417-155">Creación de un proyecto de API web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19417-155">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="19417-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="19417-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="19417-157">Vaya a **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="19417-157">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="19417-158">Seleccione el tipo de proyecto **Aplicación web de ASP.NET Core** y, luego, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="19417-158">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="19417-159">Denomine el proyecto *BooksApi* y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="19417-159">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="19417-160">Seleccione el marco de destino **.NET Core** y **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="19417-160">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="19417-161">Seleccione la plantilla de proyecto **API** y, luego, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="19417-161">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="19417-162">Visite la [galería de NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) para determinar la última versión estable del controlador .NET para MongoDB.</span><span class="sxs-lookup"><span data-stu-id="19417-162">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="19417-163">En la ventana **Consola del Administrador de paquetes**, desplácese hasta la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="19417-163">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="19417-164">Ejecute el siguiente comando para instalar el controlador .NET para MongoDB:</span><span class="sxs-lookup"><span data-stu-id="19417-164">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="19417-165">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19417-165">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="19417-166">Ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="19417-166">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="19417-167">Se genera un nuevo proyecto de API web de ASP.NET Core destinado a .NET Core, que puede abrir en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="19417-167">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="19417-168">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo en el que se le indicará que **faltan los activos necesarios para compilar y depurar en "RazonPagesMovie" y, luego, si quiere agregarlos**.</span><span class="sxs-lookup"><span data-stu-id="19417-168">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="19417-169">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="19417-169">Select **Yes**.</span></span>
1. <span data-ttu-id="19417-170">Visite la [galería de NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) para determinar la última versión estable del controlador .NET para MongoDB.</span><span class="sxs-lookup"><span data-stu-id="19417-170">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="19417-171">Abra **Terminal integrado** y navegue hasta la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="19417-171">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="19417-172">Ejecute el siguiente comando para instalar el controlador .NET para MongoDB:</span><span class="sxs-lookup"><span data-stu-id="19417-172">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="19417-173">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="19417-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="19417-174">Vaya a **Archivo** > **Nueva solución** >  **.NET Core** > **Aplicación**.</span><span class="sxs-lookup"><span data-stu-id="19417-174">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="19417-175">Seleccione la plantilla de proyecto de C# **API web ASP.NET Core** y, luego, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="19417-175">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="19417-176">Seleccione **.NET Core 2.2** en la lista desplegable **Plataforma de destino** y, luego, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="19417-176">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="19417-177">Escriba *BooksApi* en **Nombre del proyecto** y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="19417-177">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="19417-178">En el panel **Explorador de soluciones**, haga clic con el botón derecho en el nodo **Dependencias** del proyecto y seleccione **Agregar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="19417-178">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="19417-179">Escriba *MongoDB.Driver* en el cuadro de búsqueda, seleccione el paquete *MongoDB.Driver* y, luego, **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="19417-179">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="19417-180">Seleccione el botón **Aceptar** del cuadro de diálogo **Aceptación de la licencia**.</span><span class="sxs-lookup"><span data-stu-id="19417-180">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="19417-181">Adición de un modelo de entidad</span><span class="sxs-lookup"><span data-stu-id="19417-181">Add an entity model</span></span>

1. <span data-ttu-id="19417-182">Agregue un directorio *Modelos* a la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="19417-182">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="19417-183">Agregue una clase `Book` al directorio *Modelos* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="19417-183">Add a `Book` class to the *Models* directory with the following code:</span></span>

    ```csharp
    using MongoDB.Bson;
    using MongoDB.Bson.Serialization.Attributes;
    
    namespace BooksApi.Models
    {
        public class Book
        {
            [BsonId]
            [BsonRepresentation(BsonType.ObjectId)]
            public string Id { get; set; }
    
            [BsonElement("Name")]
            public string BookName { get; set; }
    
            public decimal Price { get; set; }
    
            public string Category { get; set; }
    
            public string Author { get; set; }
        }
    }
    ```

    <span data-ttu-id="19417-184">En la clase anterior, se requiere la propiedad `Id`</span><span class="sxs-lookup"><span data-stu-id="19417-184">In the preceding class, the `Id` property:</span></span>
    
    * <span data-ttu-id="19417-185">para asignar el objeto de Common Language Runtime (CLR) a la colección de MongoDB.</span><span class="sxs-lookup"><span data-stu-id="19417-185">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="19417-186">Se anota con [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) para designar esta propiedad como clave principal del documento.</span><span class="sxs-lookup"><span data-stu-id="19417-186">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="19417-187">Se anota con [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) para permitir que el parámetro pase como tipo `string` en lugar de como una estructura [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm).</span><span class="sxs-lookup"><span data-stu-id="19417-187">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="19417-188">Mongo controla la conversión de `string` a `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="19417-188">Mongo handles the conversion from `string` to `ObjectId`.</span></span>
    
    <span data-ttu-id="19417-189">La propiedad `BookName` se anota con el atributo [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm).</span><span class="sxs-lookup"><span data-stu-id="19417-189">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="19417-190">El valor `Name` del atributo representa el nombre de propiedad en la colección de MongoDB.</span><span class="sxs-lookup"><span data-stu-id="19417-190">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="19417-191">Adición de un modelo configuración</span><span class="sxs-lookup"><span data-stu-id="19417-191">Add a configuration model</span></span>

1. <span data-ttu-id="19417-192">Agregue los siguientes valores de configuración de base de datos a *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="19417-192">Add the following database configuration values to *appsettings.json*:</span></span>

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="19417-193">Agregue un archivo *BookstoreDatabaseSettings.cs* al directorio *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="19417-193">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    <span data-ttu-id="19417-194">La clase anterior `BookstoreDatabaseSettings` se utiliza para almacenar los valores de propiedad `BookstoreDatabaseSettings` del archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="19417-194">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="19417-195">Los nombres de las propiedades de JSON y C# son iguales para facilitar el proceso de asignación.</span><span class="sxs-lookup"><span data-stu-id="19417-195">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="19417-196">Agregue el código resaltado siguiente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="19417-196">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

    <span data-ttu-id="19417-197">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="19417-197">In the preceding code:</span></span>

    * <span data-ttu-id="19417-198">La instancia de configuración a la que la sección `BookstoreDatabaseSettings` del archivo *appsettings.json* enlaza está registrada en el contenedor de inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="19417-198">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="19417-199">Por ejemplo, una propiedad `ConnectionString` del objeto `BookstoreDatabaseSettings` se rellena con la propiedad `BookstoreDatabaseSettings:ConnectionString` en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="19417-199">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="19417-200">La interfaz `IBookstoreDatabaseSettings` se registra en la inserción de dependencias con una [duración de servicio](xref:fundamentals/dependency-injection#service-lifetimes) de tipo singleton.</span><span class="sxs-lookup"><span data-stu-id="19417-200">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="19417-201">Cuando se inserta, la instancia de la interfaz se resuelve en un objeto `BookstoreDatabaseSettings`.</span><span class="sxs-lookup"><span data-stu-id="19417-201">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="19417-202">Agregue el código siguiente en la parte superior del archivo *Startup.cs* para resolver las referencias a `BookstoreDatabaseSettings` y `IBookstoreDatabaseSettings`:</span><span class="sxs-lookup"><span data-stu-id="19417-202">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="19417-203">Adición de un servicio de operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="19417-203">Add a CRUD operations service</span></span>

1. <span data-ttu-id="19417-204">Agregue un directorio *Servicios* a la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="19417-204">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="19417-205">Agregue una clase `BookService` al directorio *Servicios* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="19417-205">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    <span data-ttu-id="19417-206">En el código anterior, se recuperó una instancia de `IBookstoreDatabaseSettings` de la inserción de dependencias mediante la inserción de un constructor.</span><span class="sxs-lookup"><span data-stu-id="19417-206">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="19417-207">Esta técnica proporciona acceso a los valores de configuración de *appsettings.json* que se agregaron en la sección [Adición de un modelo de configuración](#add-a-configuration-model).</span><span class="sxs-lookup"><span data-stu-id="19417-207">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="19417-208">Agregue el código resaltado siguiente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="19417-208">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

    <span data-ttu-id="19417-209">En el código anterior, la clase `BookService` se registra con inserción de dependencias para admitir la inserción del constructor en las clases de consumo.</span><span class="sxs-lookup"><span data-stu-id="19417-209">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="19417-210">La duración de servicio de tipo singleton es la más adecuada porque `BookService` toma una dependencia directa sobre `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="19417-210">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="19417-211">Según las [instrucciones oficiales de reutilización de cliente Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` debe registrarse en la inserción de dependencias con una duración de servicio de tipo singleton.</span><span class="sxs-lookup"><span data-stu-id="19417-211">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="19417-212">Agregue el código siguiente en la parte superior del archivo *Startup.cs* para resolver la referencia a `BookService`:</span><span class="sxs-lookup"><span data-stu-id="19417-212">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="19417-213">La clase`BookService` usa los miembros `MongoDB.Driver` siguientes para realizar operaciones CRUD en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="19417-213">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="19417-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm): lee la instancia del servidor para realizar operaciones de base de datos.</span><span class="sxs-lookup"><span data-stu-id="19417-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="19417-215">Se proporciona la cadena de conexión de MongoDB al constructor de esta clase:</span><span class="sxs-lookup"><span data-stu-id="19417-215">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="19417-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm): representa la base de datos de Mongo para realizar operaciones.</span><span class="sxs-lookup"><span data-stu-id="19417-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="19417-217">Este tutorial usa el método genérico [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) en la interfaz para tener acceso a los datos de una colección específica.</span><span class="sxs-lookup"><span data-stu-id="19417-217">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="19417-218">Realice las operaciones CRUD en la colección después de llamar a este método.</span><span class="sxs-lookup"><span data-stu-id="19417-218">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="19417-219">En la llamada al método `GetCollection<TDocument>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="19417-219">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="19417-220">`collection` representa el nombre de la colección.</span><span class="sxs-lookup"><span data-stu-id="19417-220">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="19417-221">`TDocument` representa el tipo de objeto CLR almacenado en la colección.</span><span class="sxs-lookup"><span data-stu-id="19417-221">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="19417-222">`GetCollection<TDocument>(collection)` devuelve un objeto [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) que representa la colección.</span><span class="sxs-lookup"><span data-stu-id="19417-222">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="19417-223">En este tutorial, se invocan los métodos siguientes en la colección:</span><span class="sxs-lookup"><span data-stu-id="19417-223">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="19417-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm): elimina un único documento que cumpla los criterios de búsqueda proporcionados.</span><span class="sxs-lookup"><span data-stu-id="19417-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="19417-225">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm): devuelve todos los documentos de la colección que cumplen los criterios de búsqueda indicados.</span><span class="sxs-lookup"><span data-stu-id="19417-225">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="19417-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm): inserta el objeto proporcionado como un nuevo documento en la colección.</span><span class="sxs-lookup"><span data-stu-id="19417-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="19417-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm): reemplaza un único documento que cumpla los criterios de búsqueda indicados por el objeto proporcionado.</span><span class="sxs-lookup"><span data-stu-id="19417-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="19417-228">Incorporación de un controlador</span><span class="sxs-lookup"><span data-stu-id="19417-228">Add a controller</span></span>

<span data-ttu-id="19417-229">Agregue una clase `BooksController` al directorio *Controladores* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="19417-229">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

<span data-ttu-id="19417-230">El controlador de API web anterior:</span><span class="sxs-lookup"><span data-stu-id="19417-230">The preceding web API controller:</span></span>

* <span data-ttu-id="19417-231">Usa la clase `BookService` para realizar operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="19417-231">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="19417-232">Contiene métodos de acción para admitir las solicitudes GET, POST, PUT y DELETE de HTTP.</span><span class="sxs-lookup"><span data-stu-id="19417-232">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="19417-233">Llama a <xref:System.Web.Http.ApiController.CreatedAtRoute*> en el método de acción `Create` para devolver una respuesta [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="19417-233">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="19417-234">El código de estado 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="19417-234">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="19417-235">`CreatedAtRoute` también agrega un encabezado `Location` a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="19417-235">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="19417-236">El encabezado `Location` especifica el identificador URI del libro recién creado.</span><span class="sxs-lookup"><span data-stu-id="19417-236">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="19417-237">Prueba de la API web</span><span class="sxs-lookup"><span data-stu-id="19417-237">Test the web API</span></span>

1. <span data-ttu-id="19417-238">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="19417-238">Build and run the app.</span></span>

1. <span data-ttu-id="19417-239">Vaya a `http://localhost:<port>/api/books` para probar el método de acción `Get` sin parámetros del controlador.</span><span class="sxs-lookup"><span data-stu-id="19417-239">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="19417-240">Se muestra la siguiente respuesta JSON:</span><span class="sxs-lookup"><span data-stu-id="19417-240">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="19417-241">Vaya a `http://localhost:<port>/api/books/{id here}` para probar el método de acción `Get` sobrecargado del controlador.</span><span class="sxs-lookup"><span data-stu-id="19417-241">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="19417-242">Se muestra la siguiente respuesta JSON:</span><span class="sxs-lookup"><span data-stu-id="19417-242">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"{ID}",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="19417-243">Configuración de las opciones de serialización de JSON</span><span class="sxs-lookup"><span data-stu-id="19417-243">Configure JSON serialization options</span></span>

<span data-ttu-id="19417-244">Hay dos detalles que cambiar sobre las respuestas JSON devueltas en la sección [Prueba de la API web](#test-the-web-api):</span><span class="sxs-lookup"><span data-stu-id="19417-244">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="19417-245">Las mayúsculas y minúsculas Camel predeterminadas de los nombres de propiedad se deben cambiar para que coincidan con el uso de mayúsculas y minúsculas de Pascal de los nombres de propiedad del objeto CLR.</span><span class="sxs-lookup"><span data-stu-id="19417-245">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="19417-246">La propiedad `bookName` se debe devolver como `Name`.</span><span class="sxs-lookup"><span data-stu-id="19417-246">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="19417-247">Para satisfacer los requisitos anteriores, realice los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="19417-247">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="19417-248">En `Startup.ConfigureServices`, cambie el código resaltado siguiente en la llamada al método `AddMvc`:</span><span class="sxs-lookup"><span data-stu-id="19417-248">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

    <span data-ttu-id="19417-249">Con el cambio anterior, los nombres de propiedad de la respuesta JSON serializada de la API web coinciden con sus nombres de propiedad correspondientes en el tipo de objeto CLR.</span><span class="sxs-lookup"><span data-stu-id="19417-249">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="19417-250">Por ejemplo, la propiedad `Author` de la clase `Book` se serializa como `Author`.</span><span class="sxs-lookup"><span data-stu-id="19417-250">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="19417-251">En *Models/Book.cs*, anote la propiedad `BookName` con el atributo [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) siguiente:</span><span class="sxs-lookup"><span data-stu-id="19417-251">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

    <span data-ttu-id="19417-252">El valor `Name` del atributo `[JsonProperty]` representa el nombre de propiedad en la respuesta JSON serializada de la API web.</span><span class="sxs-lookup"><span data-stu-id="19417-252">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="19417-253">Agregue el código siguiente en la parte superior del archivo *Models/Book.cs* para resolver la referencia al atributo `[JsonProperty]`:</span><span class="sxs-lookup"><span data-stu-id="19417-253">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="19417-254">Repita los pasos definidos en la sección [Prueba de la API web](#test-the-web-api).</span><span class="sxs-lookup"><span data-stu-id="19417-254">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="19417-255">Observe la diferencia en los nombres de propiedad JSON.</span><span class="sxs-lookup"><span data-stu-id="19417-255">Notice the difference in JSON property names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="19417-256">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="19417-256">Next steps</span></span>

<span data-ttu-id="19417-257">Para obtener más información sobre la creación de las API web de ASP.NET Core, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="19417-257">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="19417-258">Versión de YouTube de este artículo</span><span class="sxs-lookup"><span data-stu-id="19417-258">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
