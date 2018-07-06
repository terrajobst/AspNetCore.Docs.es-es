---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Crear un Singleton en OData v4 usar Web API 2.2 | Microsoft Docs
author: rick-anderson
description: En este tema se muestra cómo definir un singleton en un extremo de OData en Web API 2.2.
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: baccfed79949ae4efd45395bed3ad0058549cb4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830521"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="7f4f2-103">Crear un Singleton en OData v4 usar Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="7f4f2-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="7f4f2-104">por Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="7f4f2-104">by Zoe Luo</span></span>

> <span data-ttu-id="7f4f2-105">Tradicionalmente, una entidad solo se puede acceder si se encapsule dentro de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="7f4f2-106">Pero OData v4 proporciona dos opciones adicionales, Singleton y contención, ambos de los cuales admite WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="7f4f2-107">En este artículo se muestra cómo definir un singleton en un extremo de OData en Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="7f4f2-108">Para obtener información sobre un singleton de qué es y cómo puede beneficiarse de usarlo, consulte [mediante un singleton para definir la entidad especial](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f4f2-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="7f4f2-109">Para crear un punto de conexión de OData V4 en Web API, consulte [crear una mediante ASP.NET Web API 2.2 de OData v4 extremo](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="7f4f2-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="7f4f2-110">Vamos a crear un singleton en el proyecto de API Web mediante el modelo de datos siguientes:</span><span class="sxs-lookup"><span data-stu-id="7f4f2-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Modelo de datos](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="7f4f2-112">Un valor singleton denominado `Umbrella` se definirán según tipo `Company`y un conjunto con nombre de entidades `Employees` se definirán según tipo `Employee`.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="7f4f2-113">La solución que se usa en este tutorial se puede descargar desde [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="7f4f2-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="7f4f2-114">Definir el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="7f4f2-114">Define the data model</span></span>

1. <span data-ttu-id="7f4f2-115">Definir los tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="7f4f2-116">Generar el modelo EDM a partir de los tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="7f4f2-117">En este caso, `builder.Singleton<Company>("Umbrella")` indica el generador de modelos para crear un valor singleton denominado `Umbrella` en el modelo EDM.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="7f4f2-118">Los metadatos generados tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f4f2-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="7f4f2-119">Desde los metadatos, vemos que la propiedad de navegación `Company` en el `Employees` está enlazado el conjunto de entidades para el singleton `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="7f4f2-120">El enlace se realiza automáticamente `ODataConventionModelBuilder`, puesto que sólo `Umbrella` tiene la `Company` tipo.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="7f4f2-121">Si hay alguna ambigüedad en el modelo, puede usar `HasSingletonBinding` para enlazar explícitamente una propiedad de navegación en un singleton; `HasSingletonBinding` tiene el mismo efecto que usar el `Singleton` atributo en la definición de tipo CLR:</span><span class="sxs-lookup"><span data-stu-id="7f4f2-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="7f4f2-122">Definir el controlador de singleton</span><span class="sxs-lookup"><span data-stu-id="7f4f2-122">Define the singleton controller</span></span>

<span data-ttu-id="7f4f2-123">Al igual que el controlador de EntitySet, el controlador de singleton se hereda de `ODataController`, y debe ser el nombre del controlador singleton `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="7f4f2-124">Con el fin de controlar los distintos tipos de solicitudes, acciones deben estar previamente definido en el controlador.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="7f4f2-125">**Enrutamiento mediante atributos** está habilitada de forma predeterminada en WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="7f4f2-126">Por ejemplo, para definir una acción para controlar consultar `Revenue` desde `Company` mediante el enrutamiento mediante atributos, use lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f4f2-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="7f4f2-127">Si no estás dispuesto a definir los atributos de cada acción, simplemente definen las acciones siguientes [convenciones de enrutamiento de OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="7f4f2-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="7f4f2-128">Dado que una clave no es necesaria para realizar consultas en un singleton, las acciones definidas en el controlador de singleton son ligeramente diferentes de las acciones definidos en el controlador de entityset.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="7f4f2-129">Como referencia, las firmas de método para cada definición de la acción en el controlador de singleton se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="7f4f2-130">Básicamente, esto es todo lo que necesita hacer en el lado del servicio.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="7f4f2-131">El [proyecto de ejemplo](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene todo el código para la solución y el cliente de OData que se muestra cómo usar el singleton.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="7f4f2-132">El cliente se crea siguiendo los pasos descritos en [crear una aplicación de cliente de OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="7f4f2-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="7f4f2-133">.</span><span class="sxs-lookup"><span data-stu-id="7f4f2-133">.</span></span> 

<span data-ttu-id="7f4f2-134">*Gracias a Leo Hu para el contenido original de este artículo.*</span><span class="sxs-lookup"><span data-stu-id="7f4f2-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
