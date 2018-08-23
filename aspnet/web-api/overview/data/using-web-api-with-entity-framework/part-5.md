---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Crear objetos de transferencia de datos (dto) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 95075dd748f0fe4eb6d1c52d6bfe4a4576653b4c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836993"
---
<a name="create-data-transfer-objects-dtos"></a>Crear objetos de transferencia de datos (dto)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

Derecha ahora, nuestra API de web expone las entidades de base de datos al cliente. El cliente recibe los datos que se asigna directamente a las tablas de base de datos. Sin embargo, no siempre es una buena idea. A veces desea cambiar la forma de los datos que envía al cliente. Por ejemplo, puedes:

- Quite las referencias circulares (consulte la sección anterior).
- Ocultar determinadas propiedades que los clientes no deberían para ver.
- Omitir algunas de las propiedades con el fin de reducir el tamaño de la carga.
- Eliminar el formato de gráficos de objetos que contienen objetos anidados, para que sean más conveniente para los clientes.
- Evite "exceso" las vulnerabilidades por publicación. (Consulte [validación del modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para obtener una explicación de la publicación excesiva.)
- Desacoplar del nivel de servicio de su capa de base de datos.

Para lograr esto, puede definir un *objeto de transferencia de datos* (DTO). Un DTO es un objeto que define cómo se enviarán los datos a través de la red. Veamos cómo funciona con la entidad del libro. En la carpeta Models, agregue dos clases DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

El `BookDetailDTO` clase incluye todas las propiedades del modelo de libro, salvo que `AuthorName` es una cadena que se va a contener el nombre del autor. El `BookDTO` clase contiene un subconjunto de propiedades de `BookDetailDTO`.

A continuación, reemplace los dos métodos GET en el `BooksController` (clase), con las versiones que devuelven objetos dto. Vamos a usar LINQ **seleccione** instrucción para convertir de entidades de libro en el dto.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Este es el SQL generado por el nuevo `GetBooks` método. Puede ver que EF traduce el LINQ **seleccione** en una instrucción SELECT de SQL.

[!code-sql[Main](part-5/samples/sample3.sql)]

Por último, modifique el `PostBook` método devuelva un DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> En este tutorial, estamos convirtiendo los dto manualmente en el código. Otra opción consiste en usar una biblioteca como [AutoMapper](http://automapper.org/) que controla la conversión automáticamente.
> 
> [!div class="step-by-step"]
> [Anterior](part-4.md)
> [Siguiente](part-6.md)
