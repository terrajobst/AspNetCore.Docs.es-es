---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Validación con un nivel de servicio (C#) | Microsoft Docs
author: StephenWalther
description: Obtenga información sobre cómo mover la lógica de validación fuera de sus acciones de controlador y en una capa de servicio independiente. En este tutorial, Stephen Walther explica cómo se...
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: b0cea00ae59a15d5472e9cd9d91d86ca65f781e9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833716"
---
<a name="validating-with-a-service-layer-c"></a>Validación con un nivel de servicio (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Obtenga información sobre cómo mover la lógica de validación fuera de sus acciones de controlador y en una capa de servicio independiente. En este tutorial, Stephen Walther explica cómo puede mantener una separación de preocupaciones sharp aislando la capa de servicio de su capa de controlador.


El objetivo de este tutorial es describir un método para realizar la validación en una aplicación ASP.NET MVC. En este tutorial, obtendrá información sobre cómo mover la lógica de validación fuera de los controladores y a una capa de servicio independiente.

## <a name="separating-concerns"></a>Separación de preocupaciones

Al compilar una aplicación ASP.NET MVC, no debe colocar la lógica de la base de datos dentro de sus acciones de controlador. Mezclar la lógica de la base de datos y el controlador hace que su aplicación más difícil de mantener con el tiempo. La recomendación es colocar toda la lógica de base de datos en una capa de repositorio independiente.

Por ejemplo, el listado 1 contiene un repositorio simple denominado el ProductRepository. El repositorio de producto contiene todo el código de acceso de datos para la aplicación. La lista también incluye la interfaz IProductRepository que implementa el repositorio de producto.

**Listado 1--Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

El controlador en el listado 2 usa la capa de repositorio en sus Index() y Create() acciones. Tenga en cuenta que este controlador no contiene ninguna lógica de la base de datos. Creación de una capa de repositorio le permite mantener una separación clara de intereses. Los controladores son responsables de la lógica de control de flujo de aplicación y el repositorio es responsable de la lógica de acceso a datos.

**Listado 2 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Creación de una capa de servicio

Por lo tanto, pertenece un controlador de lógica de control de flujo de aplicación y lógica de acceso a datos pertenece a un repositorio. En ese caso, ¿dónde coloca su lógica de validación? Una opción consiste en colocar la lógica de validación en un *capa de servicio*.

Un nivel de servicio es una capa adicional en una aplicación de ASP.NET MVC que Media en la comunicación entre un controlador y la capa de repositorio. La capa de servicio contiene la lógica de negocios. En concreto, contiene lógica de validación.

Por ejemplo, la capa de servicio de producto en el listado 3 tiene un método CreateProduct(). El método CreateProduct() llama al método de ValidateProduct() para validar un nuevo producto antes de pasar el producto en el repositorio de producto.

**Listado 3 - Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Se actualizó el controlador de producto en el listado 4 para usar la capa de servicio en lugar de la capa de repositorio. El nivel de controlador se comunica con el nivel de servicio. La capa de servicio se comunica con la capa de repositorio. Cada capa tiene una responsabilidad independiente.

**Listado 4 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Tenga en cuenta que el servicio de producto se crea en el constructor del controlador del producto. Cuando se crea el servicio de producto, el diccionario de Estados del modelo se pasa al servicio. El servicio del producto usa el estado de modelo para pasar mensajes de error de validación volver al controlador.

## <a name="decoupling-the-service-layer"></a>Desacoplamiento de la capa de servicio

Nos hemos podido aislar el controlador y los niveles de servicio en un aspecto. El controlador y los niveles de servicio se comunican a través de estado del modelo. En otras palabras, la capa de servicio tiene una dependencia en una determinada característica de ASP.NET MVC framework.

Queremos aislar la capa de servicio desde nuestro nivel de controlador tanto como sea posible. En teoría, podremos usar la capa de servicio con cualquier tipo de aplicación y no solo en una aplicación ASP.NET MVC. Por ejemplo, en el futuro, podríamos queremos crear un front-end para nuestra aplicación de WPF. Debemos encontramos una manera de quitar la dependencia de ASP.NET MVC estado del modelo de la capa de servicio.

En el listado 5 se actualizó el nivel de servicio para que ya no utiliza el estado de modelo. En su lugar, utiliza cualquier clase que implementa la interfaz IValidationDictionary.

**Listado 5 - Models\ProductService.cs (desacoplado)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

La interfaz IValidationDictionary se define en el listado 6. Esta interfaz simple tiene un solo método y una sola propiedad.

**Listado 6 - Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

La clase en la lista 7, con el nombre de la clase ModelStateWrapper, implementa la interfaz IValidationDictionary. Puede crear una instancia de la clase ModelStateWrapper pasando un diccionario de Estados del modelo al constructor.

**Listado 7 - Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Por último, el controlador actualizado del listado 8 usa el ModelStateWrapper al crear la capa de servicio en su constructor.

**Listado 8 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

Mediante el IValidationDictionary interfaz y la clase ModelStateWrapper nos permite aislar completamente nuestro nivel de servicio desde nuestro nivel de controlador. La capa de servicio ya no es dependiente de estado del modelo. Puede pasar cualquier clase que implementa la interfaz IValidationDictionary a la capa de servicio. Por ejemplo, una aplicación de WPF podría implementar la interfaz IValidationDictionary con una clase de colección simple.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era comentar un método para realizar la validación en una aplicación ASP.NET MVC. En este tutorial, aprendió a mover toda la lógica de validación fuera de los controladores y a una capa de servicio independiente. También ha aprendido a aislar la capa de servicio de su capa de controlador mediante la creación de una clase ModelStateWrapper.

> [!div class="step-by-step"]
> [Anterior](validating-with-the-idataerrorinfo-interface-cs.md)
> [Siguiente](validation-with-the-data-annotation-validators-cs.md)
