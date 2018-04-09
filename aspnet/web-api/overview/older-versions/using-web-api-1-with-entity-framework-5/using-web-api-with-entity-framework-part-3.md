---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Crear un controlador de administración | Documentos de Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a>Parte 3: Crear un controlador de administración
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Agregar un controlador de administración

En esta sección, vamos a agregar un controlador de API Web que admita CRUD (crear, leer, actualizar y eliminar) operaciones sobre productos. El controlador utilizará Entity Framework para comunicarse con el nivel de base de datos. Sólo los administradores podrán usar este controlador. Los clientes tendrán acceso a los productos a través de otro controlador.

En el Explorador de soluciones, haga clic en la carpeta de controladores. Seleccione **agregar** y, a continuación, **controlador**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

En el **Agregar controlador** cuadro de diálogo, el nombre del controlador `AdminController`. En **plantilla**, seleccione &quot;controlador API con acciones de lectura/escritura, mediante Entity Framework&quot;. En **clase modelo**, seleccione "Product (ProductStore.Models)". En **contexto de datos**, seleccione "&lt;nuevo contexto de datos&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Si el **clase modelo** desplegable no muestra todas las clases de modelo, asegúrese de que ha compilado el proyecto. Entity Framework usa la reflexión, por lo que necesita el ensamblado compilado.


Al seleccionar "&lt;nuevo contexto de datos&gt;" se abrirá la **nuevo contexto de datos** cuadro de diálogo. El contexto de datos el nombre `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Haga clic en **Aceptar** para descartar la **nuevo contexto de datos** cuadro de diálogo. En el **Agregar controlador** cuadro de diálogo, haga clic en **agregar**.

Aquí es lo que se agregó al proyecto:

- Una clase denominada `OrdersContext` que se deriva de **DbContext**. Esta clase proporciona la unión entre los modelos POCO y la base de datos.
- Un controlador de API Web denominado `AdminController`. Este controlador admite operaciones CRUD en `Product` instancias. Usa el `OrdersContext` clase para comunicarse con Entity Framework.
- Una nueva cadena de conexión de base de datos en el archivo Web.config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Abra el archivo OrdersContext.cs. Tenga en cuenta que el constructor especifica el nombre de la cadena de conexión de base de datos. Este nombre hace referencia a la cadena de conexión que se ha agregado al archivo Web.config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Agregue las propiedades siguientes a la clase `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet** representa un conjunto de entidades que se pueden consultar. Esta es la lista completa para el `OrdersContext` clase:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

La `AdminController` clase define cinco métodos que implementan la funcionalidad básica de CRUD. Cada método se corresponde con un URI que el cliente puede invocar:

| Método de controlador | Descripción | Identificador URI | Método HTTP |
| --- | --- | --- | --- |
| GetProducts | Obtiene todos los productos. | API y productos | GET |
| GetProduct | Busca un producto por identificador. | api/products/*id* | GET |
| PutProduct | Actualiza un producto. | api/products/*id* | PUT |
| PostProduct | Crea un nuevo producto. | API y productos | EXPONER |
| DeleteProduct | Elimina un producto. | api/products/*id* | SUPRIMIR |

Cada método llama a `OrdersContext` para consultar la base de datos. Llamar los métodos que modifican la colección (PUT, POST y DELETE) `db.SaveChanges` para conservar los cambios a la base de datos. Los controladores se crean por cada solicitud HTTP y, a continuación, elimina, por lo que es necesario conservar los cambios antes de que un método devuelve.

## <a name="add-a-database-initializer"></a>Agregar a un inicializador de base de datos

Entity Framework tiene una característica interesante que permite rellenar la base de datos en el inicio y vuelva a crear automáticamente la base de datos cada vez que cambian los modelos. Esta característica es útil durante el desarrollo, porque siempre hay algunos datos de prueba, incluso si cambia los modelos.

En el Explorador de soluciones, haga clic en la carpeta modelos y cree una nueva clase denominada `OrdersContextInitializer`. Pegue la siguiente implementación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Al heredar de la **DropCreateDatabaseIfModelChanges** (clase), se está indicando Entity Framework para quitar la base de datos cada vez que se modifica las clases del modelo. Cuando Entity Framework crea o vuelve a crear la base de datos, se llama a la **inicialización** método para rellenar las tablas. Usamos el **inicialización** método para agregar un pedido de ejemplo además de algunos productos de ejemplo.

Esta característica es muy útil para pruebas, pero no use la **DropCreateDatabaseIfModelChanges** clase en producción, porque podría perder los datos si alguien cambie una clase de modelo.

A continuación, abra Global.asax y agregue el código siguiente a la **aplicación\_iniciar** método:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Envía una solicitud al controlador

En este momento, que no hemos escrito el código de cliente, pero puede invocar la API mediante un explorador web o una depuración HTTP como herramienta de web [Fiddler](http://www.fiddler2.com/fiddler2/). En Visual Studio, presione F5 para iniciar la depuración. Se abrirá el explorador web en `http://localhost:*portnum*/`, donde *portnum* es un número de puerto.

Enviar una solicitud HTTP a "`http://localhost:*portnum*/api/admin`. La primera solicitud puede ser tarda mucho en completarse, porque el marco de trabajo de Entify necesita crear e inicializar la base de datos. La respuesta debe ser algo similar a lo siguiente:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-2.md)
> [Siguiente](using-web-api-with-entity-framework-part-4.md)
