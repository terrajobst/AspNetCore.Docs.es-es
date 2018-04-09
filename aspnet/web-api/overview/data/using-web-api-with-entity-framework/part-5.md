---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Crear objetos de transferencia de datos (dto) | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="create-data-transfer-objects-dtos"></a>Crear objetos de transferencia de datos (dto)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](https://github.com/MikeWasson/BookService)

Derecha, nuestra API web expone las entidades de base de datos al cliente. El cliente recibe los datos que se asigna directamente a las tablas de base de datos. Sin embargo, no siempre es una buena idea. A veces desea cambiar la forma de los datos que envía al cliente. Por ejemplo, puedes:

- Quite las referencias circulares (vea la sección anterior).
- Ocultar determinadas propiedades que los clientes no deberían para ver.
- Omitir algunas propiedades para reducir el tamaño de carga.
- Recopila los gráficos de objetos que contienen objetos anidados, para que sean más cómodo para los clientes.
- Evite "contabilización exceso" vulnerabilidades. (Consulte [validación del modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para obtener una explicación del registro excesivo.)
- Desacoplar el nivel de servicio de la capa de base de datos.

Para ello, puede definir un *objeto de transferencia de datos* (DTO). Un DTO es un objeto que define cómo se enviará los datos a través de la red. Veamos cómo funciona con la entidad del libro. En la carpeta Models, agregue dos clases DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

El `BookDetailDTO` clase incluye todas las propiedades del modelo del libro, salvo que `AuthorName` es una cadena que contendrá el nombre del autor. El `BookDTO` clase contiene un subconjunto de propiedades de `BookDetailDTO`.

A continuación, reemplace los dos métodos GET en el `BooksController` (clase), con las versiones que devuelven dto. Vamos a usar LINQ **seleccione** instrucción que se va a convertir de entidades de libro en dto.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Este es el código SQL generado por el nuevo `GetBooks` método. Puede ver que EF traduce LINQ **seleccione** en una instrucción SELECT de SQL.

[!code-sql[Main](part-5/samples/sample3.sql)]

Por último, modifique el `PostBook` método devuelva un DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> En este tutorial, nos estamos se convierte en dto manualmente en el código. Otra opción consiste en utilizar una biblioteca como [AutoMapper](http://automapper.org/) que controla la conversión automáticamente.
> 
> [!div class="step-by-step"]
> [Anterior](part-4.md)
> [Siguiente](part-6.md)
