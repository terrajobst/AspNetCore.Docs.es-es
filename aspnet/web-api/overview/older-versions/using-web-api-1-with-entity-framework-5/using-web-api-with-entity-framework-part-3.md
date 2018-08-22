---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Creación de un controlador de administración | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7ad0ec27021514b447e569e479a9e9127e3f75fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835902"
---
<a name="part-3-creating-an-admin-controller"></a>Parte 3: Creación de un controlador de administración
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Agregar un controlador de administración

En esta sección, vamos a agregar un controlador de API Web que admita CRUD (crear, leer, actualizar y eliminar) las operaciones en los productos. El controlador usará Entity Framework para comunicarse con la capa de base de datos. Solo los administradores podrán usar este controlador. Los clientes tendrán acceso a los productos a través de otro controlador.

En el Explorador de soluciones, haga clic en la carpeta Controllers. Seleccione **agregar** y, a continuación, **controlador**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

En el **Agregar controlador** cuadro de diálogo, el nombre del controlador `AdminController`. En **plantilla**, seleccione &quot;controlador API con acciones de lectura/escritura, usando Entity Framework&quot;. En **clase modelo**, seleccione "Product (ProductStore.Models)". En **contexto de datos**, seleccione "&lt;nuevo contexto de datos&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Si el **clase modelo** desplegable no muestra todas las clases de modelo, asegúrese de que ha compilado el proyecto. Entity Framework usa la reflexión, por lo que necesita el ensamblado compilado.


Seleccione "&lt;nuevo contexto de datos&gt;" se abrirá el **nuevo contexto de datos** cuadro de diálogo. Nombre del contexto de datos `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Haga clic en **Aceptar** para descartar el **nuevo contexto de datos** cuadro de diálogo. En el **Agregar controlador** cuadro de diálogo, haga clic en **agregar**.

Aquí es lo que se acaban de agregar al proyecto:

- Una clase denominada `OrdersContext` que se deriva de **DbContext**. Esta clase proporciona la unión entre los modelos POCO y la base de datos.
- Un controlador de Web API llamado `AdminController`. Este controlador admite operaciones CRUD en `Product` instancias. Usa el `OrdersContext` clase para comunicarse con Entity Framework.
- Una nueva cadena de conexión de base de datos en el archivo Web.config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Abra el archivo OrdersContext.cs. Tenga en cuenta que el constructor especifica el nombre de la cadena de conexión de base de datos. Este nombre hace referencia a la cadena de conexión que se ha agregado al archivo Web.config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Agregue las propiedades siguientes a la clase `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Un **DbSet** representa un conjunto de entidades que se pueden consultar. Este es el listado completo para el `OrdersContext` clase:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

La `AdminController` clase define cinco métodos que implementan la funcionalidad CRUD básica. Cada método corresponde a un URI que puede invocar el cliente:

| Método de controlador | Descripción | Identificador URI | Método HTTP |
| --- | --- | --- | --- |
| GetProducts | Obtiene todos los productos. | API/products | GET |
| GetProduct | Busca un producto por identificador. | API/products/*Id.* | GET |
| PutProduct | Actualiza un producto. | API/products/*Id.* | PUT |
| PostProduct | Crea un nuevo producto. | API/products | EXPONER |
| DeleteProduct | Elimina un producto. | API/products/*Id.* | SUPRIMIR |

Cada método se llama a `OrdersContext` para consultar la base de datos. Llamar los métodos que modifican la colección (PUT, POST y DELETE) `db.SaveChanges` para conservar los cambios en la base de datos. Los controladores se crean por cada solicitud HTTP y, a continuación, elimina, por lo que es necesario conservar los cambios antes de que devuelva un método.

## <a name="add-a-database-initializer"></a>Agregar a un inicializador de base de datos

Entity Framework tiene una característica agradable que le permite rellenar la base de datos en el inicio y volver a crear automáticamente la base de datos cada vez que cambian los modelos. Esta característica es útil durante el desarrollo, porque siempre hay algunos datos de prueba, incluso si cambia los modelos.

En el Explorador de soluciones, haga clic en la carpeta modelos y cree una nueva clase denominada `OrdersContextInitializer`. Pegue la siguiente implementación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Al heredar de la **DropCreateDatabaseIfModelChanges** (clase), indicamos a Entity Framework para quitar la base de datos cada vez que se modifica las clases del modelo. Cuando Entity Framework crea o vuelve a crear la base de datos, llama a la **inicialización** método para rellenar las tablas. Usamos el **inicialización** método para agregar algunos productos de ejemplo además de un pedido de ejemplo.

Esta característica es muy útil para pruebas, pero no use la **DropCreateDatabaseIfModelChanges** clase en producción, porque podría perder los datos si alguien cambia una clase de modelo.

A continuación, abra Global.asax y agregue el código siguiente a la **aplicación\_iniciar** método:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Enviar una solicitud al controlador

En este momento, que no hemos escrito ningún código de cliente, pero puede invocar la web API mediante un explorador web o una depuración de HTTP como herramienta [Fiddler](http://www.fiddler2.com/fiddler2/). En Visual Studio, presione F5 para iniciar la depuración. Se abrirá el explorador web en `http://localhost:*portnum*/`, donde *portnum* es un número de puerto.

Enviar una solicitud HTTP a "`http://localhost:*portnum*/api/admin`. La primera solicitud puede ser lenta en completarse, porque Entify Framework necesita crear e inicializar la base de datos. La respuesta debería algo similar al siguiente:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-2.md)
> [Siguiente](using-web-api-with-entity-framework-part-4.md)
