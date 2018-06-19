---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: Crear los modelos de dominio | Documentos de Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84631494c1be266c21e5e5702182df717b1d29b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878586"
---
<a name="part-2-creating-the-domain-models"></a>Parte 2: Crear los modelos de dominio
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Agregar modelos

Hay tres maneras de enfoque de Entity Framework:

- Base de datos en primer lugar: empieza con una base de datos y Entity Framework genera el código.
- Modelo en primer lugar: empieza con un modelo visual y Entity Framework genera la base de datos y el código.
- Código de prioridad: empieza con código y Entity Framework genera la base de datos.

Estamos utilizando el enfoque basado en código, por lo que se inicie mediante la definición de los objetos de dominio como POCOs (los objetos CLR antiguos sin formato). Con el enfoque basado en código, los objetos de dominio no es necesario ningún código adicional para admitir la capa de base de datos, como las transacciones o persistencia. (En concreto, no es necesario heredar de la [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) clase.) Todavía puede usar anotaciones de datos para controlar cómo se crea el esquema de base de datos en Entity Framework.

Dado que POCOs no se mantienen las propiedades adicionales que describen [estado de la base de datos](https://msdn.microsoft.com/library/system.data.entitystate.aspx), fácilmente se pueden serializar a JSON o XML. Sin embargo, esto no significa siempre debe exponer los modelos de Entity Framework directamente a los clientes, como veremos más adelante en el tutorial.

Se creará el POCOs siguientes:

- Producto
- Orden
- OrderDetail

Para crear cada clase, haga clic en la carpeta de modelos en el Explorador de soluciones. En el menú contextual, seleccione **agregar** y, a continuación, seleccione **clase.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Agregar un `Product` clase con la implementación siguiente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Por convención, Entity Framework usa el `Id` propiedad como la clave principal y se asigna a una columna de identidad en la tabla de base de datos. Cuando se crea un nuevo `Product` instancia, no establezca un valor para `Id`, porque la base de datos genera el valor.

El **ScaffoldColumn** atributo indica a ASP.NET MVC para omitir la `Id` propiedad al generar un formulario del editor. El **necesario** atributo se utiliza para validar el modelo. Especifica que el `Name` propiedad debe ser una cadena no vacía.

Agregar la `Order` clase:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Agregar la `OrderDetail` clase:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relaciones de clave externas

Un pedido contiene muchos detalles de pedidos, y cada detalle de pedido hace referencia a un único producto. Para representar estas relaciones, la `OrderDetail` clase define propiedades denominadas `OrderId` y `ProductId`. Entity Framework deducirá que estas propiedades representan las claves externas y agregará las restricciones foreign key para la base de datos.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

El `Order` y `OrderDetail` clases también incluyen propiedades de "navegación", que contienen referencias a los objetos relacionados. Dado un pedido, puede navegar a los productos en el orden siguiendo las propiedades de navegación.

Compile el proyecto ahora. Entity Framework usa la reflexión para detectar las propiedades de los modelos, por lo que requiere un ensamblado compilado crear el esquema de base de datos.

## <a name="configure-the-media-type-formatters"></a>Configurar a los formateadores de tipo de medio

A [formateador de tipo de medio](../../formats-and-model-binding/media-formatters.md) es un objeto que serializa los datos cuando la API Web escribe el cuerpo de respuesta HTTP. Los formateadores integrados admiten la salida JSON y XML. De forma predeterminada, ambos de estos formateadores serializar todos los objetos por valor.

Serialización por valor crea un problema si un gráfico de objetos contiene las referencias circulares. Que es exactamente el caso de los `Order` y `OrderDetail` clases, porque cada uno de ellos contiene una referencia a la otra. El formateador se siguen las referencias, escribir cada objeto por valor y volver círculos. Por lo tanto, es necesario cambiar el comportamiento predeterminado.

En el Explorador de soluciones, expanda la aplicación\_carpeta y abra el archivo denominado WebApiConfig.cs. Agregue el código siguiente a la clase `WebApiConfig` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Este código establece el formateador JSON para conservar las referencias de objeto y quita completamente el formateador de XML de la canalización. (Puede configurar el formateador XML para conservar las referencias de objeto, pero es un poco más trabajo y únicamente se necesita JSON para esta aplicación. Para obtener más información, consulte [control Circular referencias a objetos](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-1.md)
> [Siguiente](using-web-api-with-entity-framework-part-3.md)
