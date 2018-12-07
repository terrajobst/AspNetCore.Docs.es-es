---
title: Compilación de API web con ASP.NET Core y MongoDB
author: pratik-khandelwal
description: En este tutorial se muestra cómo crear una API web de ASP.NET Core con una base de datos NoSQL de MongoDB.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/29/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: becf55bf94a1bfe78935013d802168a0b05dccce
ms.sourcegitcommit: 0fc89b80bb1952852ecbcf3c5c156459b02a6ceb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52618095"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>Creación de una API Web con ASP.NET Core y MongoDB

Por [Pratik Khandelwal](https://twitter.com/K2Prk) y [Scott Addie](https://twitter.com/Scott_Addie)

En este tutorial se crea una API web que realiza operaciones de creación, lectura, actualización y eliminación (CRUD) en una base de datos NoSQL de [MongoDB](https://www.mongodb.com/what-is-mongodb).

En este tutorial aprenderá a:

> [!div class="checklist"]
> * Configurar MongoDB
> * Crear una base de datos de MongoDB
> * Definir un esquema y una colección de MongoDB
> * Realizar operaciones de CRUD de MongoDB desde una API web

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.2 o posterior](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017 versión 15.9 o posterior](https://www.visualstudio.com/downloads/) con la carga de trabajo **ASP.NET y desarrollo web**
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.2 o posterior](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* [.NET Core SDK 2.2 o posterior](https://www.microsoft.com/net/download/all)
* [Visual Studio para Mac, versión 7.7 o posterior](https://www.visualstudio.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>Configurar MongoDB

Si usa Windows, MongoDB está instalado en *C:\Archivos de programa\MongoDB* de forma predeterminada. Agregue *C:\Archivos de programa\MongoDB\Server\<número_versión>\bin* a la variable de entorno `Path`. Este cambio permite el acceso a MongoDB desde cualquier lugar en el equipo de desarrollo.

Use el Shell de mongo en los pasos siguientes para crear una base de datos, hacer colecciones y almacenar documentos. Para obtener más información sobre los comandos de Shell de mongo, consulte [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell) (Trabajo con el shell de Mongo).

1. Elija un directorio en el equipo de desarrollo para almacenar los datos. Por ejemplo, *C:\BooksData* en Windows. Si no existe el directorio, créelo. El shell de mongo no crea nuevos directorios.
1. Abra un shell de comandos. Ejecute el comando siguiente para conectarse a MongoDB en el puerto predeterminado 27017. No olvide reemplazar `<data_directory_path>` por el directorio que eligió en el paso anterior.

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. Abra otra instancia del shell de comandos. Conéctese a la base de datos de prueba de forma predeterminada ejecutando el comando siguiente:

    ```console
    mongo
    ```

1. Ejecute lo siguiente en un shell de comandos:

    ```console
    use BookstoreDb
    ```

    Si aún no existe, se crea una base de datos denominada *BookstoreDb*. Si la base de datos existe, su conexión se abre para las transacciones.

1. Cree una colección `Books` con el comando siguiente:

    ```console
    db.createCollection('Books')
    ```

    Se muestra el siguiente resultado:

    ```console
    { "ok" : 1 }
    ```

1. Defina un esquema para la colección `Books` e inserte dos documentos con el comando siguiente:

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    Se muestra el siguiente resultado:

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. Vea los documentos en la base de datos mediante el comando siguiente:

    ```console
    db.Books.find({}).pretty()
    ```

    Se muestra el siguiente resultado:

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

    El esquema agrega una propiedad `_id` generada automáticamente del tipo `ObjectId` para cada documento.

La base de datos está lista. Puede empezar a crear la API web de ASP.NET Core.

## <a name="create-the-aspnet-core-web-api-project"></a>Creación de un proyecto de API web de ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Vaya a **Archivo** > **Nuevo** > **Proyecto**.
1. Seleccione **Aplicación web de ASP.NET Core**, ponga al proyecto el nombre *BooksApi*y haga clic en **Aceptar**.
1. Seleccione el marco de destino **.NET Core** y **ASP.NET Core 2.1**. Seleccione la plantilla de proyecto **API** y haga clic en **Aceptar**:
1. En la ventana **Consola del Administrador de paquetes**, desplácese hasta la raíz del proyecto. Ejecute el siguiente comando para instalar el controlador .NET para MongoDB:

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Ejecute los siguientes comandos en un shell de comandos:

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    Se genera un nuevo proyecto de API web de ASP.NET Core destinado a .NET Core, que puede abrir en Visual Studio Code.

1. Haga clic en **Sí** cuando aparezca la notificación *Required assets to build and debug are missing from 'BooksApi'. Add them?* (Faltan los activos necesarios para compilar y depurar en 'BooksApi'. ¿Desea agregarlos?).
1. Abra **Terminal integrado** y navegue hasta la raíz del proyecto. Ejecute el siguiente comando para instalar el controlador .NET para MongoDB:

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v 2.7.2
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

1. Vaya a **Archivo** > **Nueva solución** > **.NET Core** > **Aplicación**.
1. Seleccione la plantilla de proyecto de C# **API web ASP.NET Core** y haga clic en **Siguiente**.
1. Seleccione **.NET Core 2.2** en la lista desplegable **Plataforma de destino** y haga clic en **Siguiente**.
1. Escriba *BooksApi* en **Nombre del proyecto** y haga clic en **Crear**.
1. En el panel **Explorador de soluciones**, haga clic con el botón derecho en el nodo **Dependencias** del proyecto y seleccione **Agregar paquetes**.
1. Escriba *MongoDB.Driver* en el cuadro de búsqueda, seleccione el paquete *MongoDB.Driver* y haga clic en **Agregar paquete**.
1. Haga clic en el botón **Aceptar** del cuadro de diálogo **Aceptación de la licencia**.

---

## <a name="add-a-model"></a>Adición de un modelo

1. Agregue un directorio *Modelos* a la raíz del proyecto.
1. Agregue una clase `Book` al directorio *Modelos* con el código siguiente:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

En la clase anterior, se requiere la propiedad `Id` para asignar el objeto de Common Language Runtime (CLR) a la colección de MongoDB. Otras propiedades de la clase se decoran con el atributo `[BsonElement]`. El valor del atributo representa el nombre de propiedad en la colección de MongoDB.

## <a name="add-a-crud-operations-class"></a>Adición de una clase de las operaciones CRUD

1. Agregue un directorio *Servicios* a la raíz del proyecto.
1. Agregue una clase `BookService` al directorio *Servicios* con el código siguiente:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. Agregue una cadena de conexión MongoDB a *appsettings.json*:

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    Se tiene acceso a la propiedad `BookstoreDb` anterior en el constructor de clase `BookService`.

1. En `Startup.ConfigureServices`, registre la clase `BookService` con el sistema de inserción de dependencias:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    El registro del servicio anterior es necesario para admitir la inserción del constructor en las clases de consumo.

La clase`BookService` usa los miembros `MongoDB.Driver` siguientes para realizar operaciones CRUD en la base de datos:

* `MongoClient` &ndash; Lee la instancia del servidor para realizar operaciones de base de datos. Se proporciona la cadena de conexión de MongoDB al constructor de esta clase:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* `IMongoDatabase` &ndash; Representa la base de datos de Mongo para realizar operaciones. Este tutorial usa el método genérico `GetCollection<T>(collection)` de la interfaz para tener acceso a los datos de una colección específica. Las operaciones CRUD pueden realizarse en la colección después de llamar a este método. En la llamada al método `GetCollection<T>(collection)`:
  * `collection` representa el nombre de la colección.
  * `T` representa el tipo de objeto CLR almacenado en la colección.

`GetCollection<T>(collection)` devuelve un objeto `MongoCollection` que representa la colección. En este tutorial, se invocan los métodos siguientes en la colección:

* `Find<T>` &ndash; Devuelve todos los documentos de la colección que cumplen los criterios de búsqueda indicados.
* `InsertOne` &ndash; Inserta el objeto proporcionado como un nuevo documento en la colección.
* `ReplaceOne` &ndash; Reemplaza un único documento que cumpla los criterios de búsqueda indicados por el objeto proporcionado.
* `DeleteOne` &ndash; Elimina un único documento que cumpla los criterios de búsqueda proporcionados.

## <a name="add-a-controller"></a>Adición de un controlador

1. Agregue una clase `BooksController` al directorio *Controladores* con el código siguiente:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    El controlador de API web anterior:

    * Usa la clase `BookService` para realizar operaciones CRUD.
    * Contiene métodos de acción para admitir las solicitudes GET, POST, PUT y DELETE de HTTP.
1. Compile y ejecute la aplicación.
1. Vaya a `http://localhost:<port>/api/books` en el explorador. Se muestra la siguiente respuesta JSON:

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

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la creación de las API web de ASP.NET Core, consulte los siguientes recursos:

* <xref:web-api/index>
* <xref:web-api/action-return-types>
