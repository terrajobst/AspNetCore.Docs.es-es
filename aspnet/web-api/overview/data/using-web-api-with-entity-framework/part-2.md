---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Agregar modelos y controladores | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879587"
---
<a name="add-models-and-controllers"></a>Agregar modelos y controladores
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, agregará las clases de modelo que definen las entidades de base de datos. A continuación, agregará controladores de API Web que realizan operaciones CRUD en las entidades.

## <a name="add-model-classes"></a>Agregar clases de modelo

En este tutorial, vamos a crear la base de datos mediante el enfoque "Code First" a Entity Framework (EF). Con Code First, escribir clases de C# que corresponden a las tablas de base de datos y EF crea la base de datos. (Para obtener más información, consulte [enfoques de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Comenzamos definiendo nuestros objetos de dominio como POCOs (los objetos CLR antiguos sin formato). Se creará el POCOs siguientes:

- Autor
- Libro

En el Explorador de soluciones, haga clic en la carpeta Models. Seleccione **agregar**, a continuación, seleccione **clase**. Asigne a la clase el nombre `Author`.

![](part-2/_static/image1.png)

Reemplace todo el código reutilizable en Author.cs por el código siguiente.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Agregue otra clase denominada `Book`, con el código siguiente.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework utilizará estos modelos para crear tablas de base de datos. Para cada modelo, la `Id` propiedad se convertirá en la columna de clave principal de la tabla de base de datos.

En la clase del libro, el `AuthorId` define una clave externa en el `Author` tabla. (Por motivos de simplicidad, supongo que cada libro tiene un solo autor.) La clase book también contiene una propiedad de navegación para relacionado `Author`. Puede usar la propiedad de navegación para tener acceso a relacionado `Author` en el código. Quiero más información sobre propiedades de navegación en la parte 4, [control de relaciones de entidad](part-4.md).

## <a name="add-web-api-controllers"></a>Agregar controladores de API Web

En esta sección, vamos a agregar los controladores de API Web que admiten operaciones CRUD (crear, leer, actualizar y eliminar). Los controladores usará Entity Framework para comunicarse con el nivel de base de datos.

En primer lugar, puede eliminar el archivo Controllers/ValuesController.cs. Este archivo contiene un controlador de API Web de ejemplo, pero no es necesario para este tutorial.

![](part-2/_static/image2.png)

A continuación, compile el proyecto. El scaffolding de Web API usa la reflexión para buscar las clases del modelo, por lo que necesita el ensamblado compilado.

En el Explorador de soluciones, haga clic en la carpeta de controladores. Seleccione **agregar**, a continuación, seleccione **controlador**.

![](part-2/_static/image3.png)

En el **agregar scaffolding** cuadro de diálogo, seleccione "Web API 2 controlador con acciones que usan Entity Framework". Haga clic en **Agregar**.

![](part-2/_static/image4.png)

En el **Agregar controlador** cuadro de diálogo, haga lo siguiente:

1. En el **clase modelo** lista desplegable, seleccione la `Author` clase. (Si no ve que aparecen en la lista desplegable, asegúrese de que ha generado el proyecto.)
2. Consulte "Acciones de controlador asincrónico de uso".
3. Deje el nombre del controlador como &quot;AuthorsController&quot;.
4. Haga clic en más (+) situado junto a **clase de contexto de datos**.

![](part-2/_static/image5.png)

En el **nuevo contexto de datos** cuadro de diálogo, deje el nombre predeterminado y haga clic en **agregar**.

![](part-2/_static/image6.png)

Haga clic en **agregar** para completar la **Agregar controlador** cuadro de diálogo. El cuadro de diálogo agrega dos clases al proyecto:

- `AuthorsController` define un controlador de Web API. El controlador implementa la API de REST que los clientes utilizan para realizar operaciones CRUD en la lista de autores.
- `BookServiceContext` administra los objetos de entidad en tiempo de ejecución, que incluye rellenar los objetos con datos de una base de datos, el seguimiento de cambios y conservar los datos de la base de datos. Hereda de `DbContext`.

![](part-2/_static/image7.png)

En este momento, compile el proyecto de nuevo. Vaya ahora a través de los mismos pasos para agregar un controlador de API para `Book` entidades. Esta vez, seleccione `Book` para la clase de modelo y seleccione existente `BookServiceContext` clase para la clase de contexto de datos. (No crear un nuevo contexto de datos). Haga clic en **agregar** para agregar el controlador.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Anterior](part-1.md)
> [Siguiente](part-3.md)
