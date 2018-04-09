---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Validar los datos con un nivel de servicio (VB) | Documentos de Microsoft
author: StephenWalther
description: Obtenga información acerca de cómo mover la lógica de validación de las acciones de controlador y almacenarla en una capa de servicio independiente. En este tutorial, Stephen Walther explica cómo se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: bb1191b663f863bf881def620efab4f2f03edc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-a-service-layer-vb"></a>Validar los datos con un nivel de servicio (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Obtenga información acerca de cómo mover la lógica de validación de las acciones de controlador y almacenarla en una capa de servicio independiente. En este tutorial, Stephen Walther explica cómo puede mantener una separación de intereses aguda al aislar el nivel de servicio de la capa de controlador.


El objetivo de este tutorial es describir un método para realizar la validación en una aplicación ASP.NET MVC. En este tutorial, aprenderá a mover la lógica de validación de los controladores y en una capa de servicio independiente.

## <a name="separating-concerns"></a>Separación de problemas

Cuando se compila una aplicación de MVC de ASP.NET, no se debe colocar la lógica de la base de datos dentro de las acciones de controlador. Mezclar la lógica de base de datos y el controlador hace que la aplicación resulte más difícil mantener con el tiempo. La recomendación es colocar toda la lógica de base de datos en una capa de repositorio independiente.

Por ejemplo, el listado 1 contiene un repositorio simple denominado el ProductRepository. El repositorio de producto contiene todo el código de acceso de datos para la aplicación. La lista también incluye la interfaz IProductRepository que implementa el repositorio de producto.

**Lista 1 - Models\ProductRepository.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

El controlador en el listado 2 usa la capa de repositorio en sus Index() y Create() acciones. Tenga en cuenta que este controlador no contiene ninguna lógica de base de datos. Creación de una capa de repositorio le permite mantener una separación clara de intereses. Los controladores son responsables de lógica de control de flujo de aplicación y el repositorio es responsable de la lógica de acceso a datos.

**La lista 2 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>Creación de una capa de servicio

Por lo tanto, pertenece un controlador de lógica de control de flujo de aplicación y lógica de acceso a datos pertenece a un repositorio. ¿En ese caso, su ubicación lógica de validación? Una opción consiste en colocar la lógica de validación en un *capa de servicio*.

Un nivel de servicio es una capa adicional en una aplicación de ASP.NET MVC que realiza la comunicación entre un controlador y la capa de repositorio. El nivel de servicio contiene la lógica de negocios. En concreto, contiene lógica de validación.

Por ejemplo, el nivel de servicio de producto en la lista 3 tiene un método CreateProduct(). El método CreateProduct() llama al método ValidateProduct() para validar un nuevo producto antes de pasar el producto en el repositorio de producto.

**El listado 3 - Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

El controlador de producto se ha actualizado en el listado 4 para usar el nivel de servicio en lugar de la capa de repositorio. El nivel de controlador se comunica con el nivel de servicio. El nivel de servicio se comunica con la capa de repositorio. Cada capa tiene una responsabilidad independiente.

**Listado 4 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

Tenga en cuenta que el servicio de producto se crea en el constructor del controlador de producto. Cuando se crea el servicio de producto, el diccionario de Estados del modelo se pasa al servicio. El servicio de producto usa estado del modelo para devolver mensajes de error de validación al controlador.

## <a name="decoupling-the-service-layer"></a>Separar el nivel de servicio

No hemos pudimos aislar el controlador y los niveles de servicio en un sentido. El controlador y los niveles de servicio se comunican a través de estado del modelo. En otras palabras, el nivel de servicio tiene una dependencia en una característica determinada del marco de MVC de ASP.NET.

Queremos aislar el nivel de servicio de la capa de controlador tanto como sea posible. En teoría, deberíamos estar puede utilizar el nivel de servicio con cualquier tipo de aplicación y no solo una aplicación de ASP.NET MVC. Por ejemplo, en el futuro, tengamos que queremos generar un front-end para nuestra aplicación de WPF. Debemos encontramos una manera de quitar la dependencia de ASP.NET MVC estado del modelo de la capa de servicio.

En el listado 5, el nivel de servicio se ha actualizado para que ya no utiliza el estado del modelo. En su lugar, utiliza cualquier clase que implementa la interfaz IValidationDictionary.

**Listado 5 - Models\ProductService.vb (desacoplado)**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

La interfaz IValidationDictionary se define en el listado 6. Esta interfaz simple tiene un método único y una sola propiedad.

**Listado 6 - Models\IValidationDictionary.cs**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

La clase en la lista 7, con el nombre de la clase ModelStateWrapper, implementa la interfaz IValidationDictionary. Puede crear una instancia de la clase ModelStateWrapper pasando un diccionario de Estados del modelo al constructor.

**Listado 7 - Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

Por último, el controlador actualizado listado 8 usa el ModelStateWrapper al crear el nivel de servicio en su constructor.

**Enumerar 8 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

Mediante el IValidationDictionary interfaz y la clase ModelStateWrapper nos permite aislar totalmente nuestro nivel de servicio de la capa de controlador. El nivel de servicio ya no es dependiente de estado del modelo. Puede pasar cualquier clase que implementa la interfaz IValidationDictionary para el nivel de servicio. Por ejemplo, una aplicación de WPF podría implementar la interfaz IValidationDictionary con una clase de colección simple.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar un enfoque para realizar la validación en una aplicación ASP.NET MVC. En este tutorial, aprendió a todas las de la lógica de validación de los controladores y en una capa de servicio independiente mover. También aprendió a aislar su nivel de servicio de la capa de controlador mediante la creación de una clase ModelStateWrapper.

> [!div class="step-by-step"]
> [Anterior](validating-with-the-idataerrorinfo-interface-vb.md)
> [Siguiente](validation-with-the-data-annotation-validators-vb.md)
