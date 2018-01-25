---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: "Modelo de validación en ASP.NET Web API | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 45b519af4073b62c8be1ca8951e44d6cf3cbe075
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="model-validation-in-aspnet-web-api"></a>Validación del modelo en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Cuando un cliente envía datos a la API web, a menudo desea validar los datos antes de realizar cualquier procesamiento. Este artículo muestra cómo agregar anotaciones a los modelos, utilice las anotaciones para la validación de datos y controlar los errores de validación en la API web.

## <a name="data-annotations"></a>Anotaciones de datos

En ASP.NET Web API, puede utilizar atributos de la [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres para establecer reglas de validación para las propiedades en el modelo. Tenga en cuenta el siguiente modelo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Si ha usado la validación del modelo en MVC de ASP.NET, esto debería ser familiar. El **requiere** atributo indica que el `Name` propiedad no debe ser null. El **intervalo** atributo dice que `Weight` debe estar comprendido entre 0 y 999.

Suponga que un cliente envía una solicitud POST con la representación JSON siguiente:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Puede ver que el cliente no incluía el `Name` propiedad, que está marcada como necesaria. Cuando la API Web convierte el JSON en un `Product` instancia, valida el `Product` con los atributos de validación. En la acción de controlador, puede comprobar si el modelo es válido:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Validación de modelos no garantiza que los datos del cliente están seguros. Puede ser necesaria una validación adicional en otros niveles de la aplicación. (Por ejemplo, la capa de datos podría aplicar restricciones de clave externa). El tutorial [usar Web API con Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora algunos de estos problemas.

**"Debajo del registro"**: debajo del registro se produce cuando el cliente no especifica algunas propiedades. Por ejemplo, suponga que el cliente envía el siguiente:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

En este caso, el cliente no ha especificado valores para `Price` o `Weight`. El formateador JSON asigna un valor predeterminado de cero para las propiedades que faltan.

![](model-validation-in-aspnet-web-api/_static/image1.png)

El estado del modelo es válido, porque cero es un valor válido para estas propiedades. Si se trata de un problema depende de su escenario. Por ejemplo, en una operación de actualización, puede distinguir entre "cero" y "no configurada". Para obligar a los clientes para establecer un valor, convierta la propiedad que acepta valores NULL y establezca el **necesario** atributo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Registro exceso"**: un cliente también puede enviar *más* datos de lo que esperaba. Por ejemplo:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

En este caso, el JSON incluye una propiedad ("Color") que no existe en el `Product` modelo. En este caso, el formateador JSON simplemente omite este valor. (El formateador XML hace lo mismo.) Registro excesivo causa problemas si el modelo tiene propiedades que pretende ser de solo lectura. Por ejemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

No desea que los usuarios para actualizar la `IsAdmin` propiedad y elevar sus privilegios a los administradores. La estrategia más segura consiste en utilizar una clase de modelo que coincide exactamente con lo que el cliente tiene permiso para enviar:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Entrada de blog de Brad Wilson "[vs de validación de entrada. Modelo de validación en ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"tiene una buena explicación de debajo del registro y el registro excesivo. Aunque es la entrada de blog sobre ASP.NET MVC 2, siguen siendo pertinentes para la API Web los problemas.


## <a name="handling-validation-errors"></a>Control de errores de validación

API Web no automáticamente devuelve un error al cliente cuando se produce un error de validación. Depende de la acción del controlador para comprobar el estado del modelo y responder según corresponda.

También puede crear un filtro de acción para comprobar el estado del modelo antes de invoca la acción del controlador. El código siguiente muestra un ejemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Si se produce un error en la validación del modelo, este filtro devuelve una respuesta HTTP que contiene los errores de validación. En ese caso, no se invoca la acción del controlador.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Para aplicar este filtro a todos los controladores de API Web, debe agregar una instancia del filtro para la **HttpConfiguration.Filters** colección durante la configuración:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Otra opción consiste en establecer el filtro como un atributo en controladores individuales o las acciones de controlador:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
