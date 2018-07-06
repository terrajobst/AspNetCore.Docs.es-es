---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: Crear modelos de dominio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 605a3eb5d781db6b317369efe8698288d7b9f648
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802275"
---
<a name="part-2-creating-the-domain-models"></a>Parte 2: Crear modelos de dominio
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Agregar modelos

Hay tres maneras de enfoque de Entity Framework:

- Base de datos en primer lugar: empieza con una base de datos, y Entity Framework genera el código.
- Model first: empieza con un modelo visual, y Entity Framework genera la base de datos y el código.
- Code first: empieza con el código, y Entity Framework genera la base de datos.

Estamos usando el enfoque de code first, por lo que comenzamos por definir nuestros objetos de dominio como poco (objetos CLR antiguos sin formato). Con el enfoque de code first, objetos de dominio no es necesario ningún código adicional para admitir la capa de base de datos, como las transacciones ni la persistencia. (En concreto, no es necesario que se heredará la [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) clase.) Todavía puede usar anotaciones de datos para controlar cómo Entity Framework crea el esquema de base de datos.

Dado que poco no transportan las propiedades adicionales que describen [estado de la base de datos](https://msdn.microsoft.com/library/system.data.entitystate.aspx), fácilmente se pueden serializar a JSON o XML. Sin embargo, eso no significa que siempre deben exponer los modelos de Entity Framework directamente a los clientes, como veremos más adelante en el tutorial.

Vamos a crear los objetos poco siguientes:

- Producto
- Orden
- OrderDetail

Para crear cada clase, haga clic en la carpeta de modelos en el Explorador de soluciones. En el menú contextual, seleccione **agregar** y, a continuación, seleccione **clase.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Agregar un `Product` clase con la siguiente implementación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Por convención, Entity Framework usa el `Id` propiedad como clave principal y lo asigna a una columna de identidad en la tabla de base de datos. Cuando se crea un nuevo `Product` instancia, no establezca un valor para `Id`, porque la base de datos genera el valor.

El **ScaffoldColumn** atributo indica a ASP.NET MVC para omitir la `Id` propiedad cuando se genera un formulario del editor. El **necesario** atributo se utiliza para validar el modelo. Especifica que el `Name` propiedad debe ser una cadena no vacía.

Agregar el `Order` clase:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Agregar el `OrderDetail` clase:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relaciones de clave externas

Un pedido contiene numerosos detalles de pedido y cada detalle de pedido hace referencia a un solo producto. Para representar estas relaciones, la `OrderDetail` clase define propiedades denominadas `OrderId` y `ProductId`. Entity Framework deducirá que estas propiedades representan las claves externas y agregará las restricciones foreign key a la base de datos.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

El `Order` y `OrderDetail` clases también incluyen propiedades de "exploración", que contienen referencias a los objetos relacionados. Dado un pedido, puede navegar a los productos en el orden siguiendo las propiedades de navegación.

Compile el proyecto ahora. Entity Framework usa la reflexión para detectar las propiedades de los modelos, por lo que requiere un ensamblado compilado crear el esquema de base de datos.

## <a name="configure-the-media-type-formatters"></a>Configurar a los formateadores de tipo de medio

Un [formateador de tipo de medio](../../formats-and-model-binding/media-formatters.md) es un objeto que serializa los datos cuando la API Web escribe el cuerpo de respuesta HTTP. Los formateadores integrados admiten la salida JSON y XML. De forma predeterminada, ambos de estos formateadores serializar todos los objetos por valor.

Serialización por valor crea un problema si un gráfico de objetos contiene las referencias circulares. Que es exactamente el caso de los `Order` y `OrderDetail` clases, porque cada uno contiene una referencia a otro. El formateador se siguen las referencias, escribir cada objeto por valor e ir en círculos. Por lo tanto, es necesario cambiar el comportamiento predeterminado.

En el Explorador de soluciones, expanda la aplicación\_carpeta Inicio y abra el archivo WebApiConfig.cs. Agregue el código siguiente a la clase `WebApiConfig` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Este código establece el formateador JSON para conservar las referencias de objeto y quita completamente el formateador XML de la canalización. (Puede configurar el formateador XML para conservar las referencias de objeto, pero es un poco más trabajo y solo necesitamos JSON para esta aplicación. Para obtener más información, consulte [referencias a objetos de control Circular](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-1.md)
> [Siguiente](using-web-api-with-entity-framework-part-3.md)
