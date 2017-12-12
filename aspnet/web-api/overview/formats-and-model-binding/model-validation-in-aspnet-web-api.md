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
ms.openlocfilehash: dc91ddb64294e686825076d5bcc636766f2f6f01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="1c6bb-102">Validación del modelo en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1c6bb-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="1c6bb-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1c6bb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1c6bb-104">Cuando un cliente envía datos a la API web, a menudo desea validar los datos antes de realizar cualquier procesamiento.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="1c6bb-105">Este artículo muestra cómo agregar anotaciones a los modelos, utilice las anotaciones para la validación de datos y controlar los errores de validación en la API web.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1c6bb-106">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="1c6bb-106">Data Annotations</span></span>

<span data-ttu-id="1c6bb-107">En ASP.NET Web API, puede utilizar atributos de la [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) espacio de nombres para establecer reglas de validación para las propiedades en el modelo.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="1c6bb-108">Tenga en cuenta el siguiente modelo:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="1c6bb-109">Si ha usado la validación del modelo en MVC de ASP.NET, esto debería ser familiar.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="1c6bb-110">El **requiere** atributo indica que el `Name` propiedad no debe ser null.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="1c6bb-111">El **intervalo** atributo dice que `Weight` debe estar comprendido entre 0 y 999.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="1c6bb-112">Suponga que un cliente envía una solicitud POST con la representación JSON siguiente:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="1c6bb-113">Puede ver que el cliente no incluía el `Name` propiedad, que está marcada como necesaria.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="1c6bb-114">Cuando la API Web convierte el JSON en un `Product` instancia, valida el `Product` con los atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="1c6bb-115">En la acción de controlador, puede comprobar si el modelo es válido:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="1c6bb-116">Validación de modelos no garantiza que los datos del cliente están seguros.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="1c6bb-117">Puede ser necesaria una validación adicional en otros niveles de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="1c6bb-118">(Por ejemplo, la capa de datos podría aplicar restricciones de clave externa). El tutorial [usar Web API con Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora algunos de estos problemas.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="1c6bb-119">**"Debajo del registro"**: debajo del registro se produce cuando el cliente no especifica algunas propiedades.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="1c6bb-120">Por ejemplo, suponga que el cliente envía el siguiente:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="1c6bb-121">En este caso, el cliente no ha especificado valores para `Price` o `Weight`.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="1c6bb-122">El formateador JSON asigna un valor predeterminado de cero para las propiedades que faltan.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="1c6bb-123">El estado del modelo es válido, porque cero es un valor válido para estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="1c6bb-124">Si se trata de un problema depende de su escenario.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="1c6bb-125">Por ejemplo, en una operación de actualización, puede distinguir entre "cero" y "no configurada".</span><span class="sxs-lookup"><span data-stu-id="1c6bb-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="1c6bb-126">Para obligar a los clientes para establecer un valor, convierta la propiedad que acepta valores NULL y establezca el **necesario** atributo:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="1c6bb-127">**"Registro exceso"**: un cliente también puede enviar *más* datos de lo que esperaba.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="1c6bb-128">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="1c6bb-129">En este caso, el JSON incluye una propiedad ("Color") que no existe en el `Product` modelo.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="1c6bb-130">En este caso, el formateador JSON simplemente omite este valor.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="1c6bb-131">(El formateador XML hace lo mismo.) Registro excesivo causa problemas si el modelo tiene propiedades que pretende ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="1c6bb-132">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="1c6bb-133">No desea que los usuarios para actualizar la `IsAdmin` propiedad y elevar sus privilegios a los administradores.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="1c6bb-134">La estrategia más segura consiste en utilizar una clase de modelo que coincide exactamente con lo que el cliente tiene permiso para enviar:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="1c6bb-135">Entrada de blog de Brad Wilson "[vs de validación de entrada. Modelo de validación en ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"tiene una buena explicación de debajo del registro y el registro excesivo.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="1c6bb-136">Aunque es la entrada de blog sobre ASP.NET MVC 2, siguen siendo pertinentes para la API Web los problemas.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="1c6bb-137">Control de errores de validación</span><span class="sxs-lookup"><span data-stu-id="1c6bb-137">Handling Validation Errors</span></span>

<span data-ttu-id="1c6bb-138">API Web no automáticamente devuelve un error al cliente cuando se produce un error de validación.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="1c6bb-139">Depende de la acción del controlador para comprobar el estado del modelo y responder según corresponda.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="1c6bb-140">También puede crear un filtro de acción para comprobar el estado del modelo antes de invoca la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="1c6bb-141">El código siguiente muestra un ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="1c6bb-142">Si se produce un error en la validación del modelo, este filtro devuelve una respuesta HTTP que contiene los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="1c6bb-143">En ese caso, no se invoca la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="1c6bb-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="1c6bb-144">Para aplicar este filtro a todos los controladores de API Web, debe agregar una instancia del filtro para la **HttpConfiguration.Filters** colección durante la configuración:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="1c6bb-145">Otra opción consiste en establecer el filtro como un atributo en controladores individuales o las acciones de controlador:</span><span class="sxs-lookup"><span data-stu-id="1c6bb-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
