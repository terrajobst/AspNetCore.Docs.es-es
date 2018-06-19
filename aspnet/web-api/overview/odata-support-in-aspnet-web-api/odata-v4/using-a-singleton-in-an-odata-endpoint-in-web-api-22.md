---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Crear un Singleton de OData v4 usar Web API 2.2 | Documentos de Microsoft
author: rick-anderson
description: Este tema muestra cómo definir un singleton en un extremo de OData de Web API 2.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508214"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="882f0-103">Crear un Singleton de OData v4 usar Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="882f0-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="882f0-104">por Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="882f0-104">by Zoe Luo</span></span>

> <span data-ttu-id="882f0-105">Tradicionalmente, una entidad puede obtenerse solo si se encapsula dentro de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="882f0-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="882f0-106">Pero con OData v4 proporciona dos opciones adicionales, Singleton y contención, ambos de los cuales admite WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="882f0-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="882f0-107">Este artículo muestra cómo definir un singleton en un extremo de OData de Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="882f0-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="882f0-108">Para obtener información sobre qué singleton es y cómo puede beneficiarse de usarlo, vea [utilizando un singleton para definir la entidad especial](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="882f0-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="882f0-109">Para crear un extremo de OData V4 en Web API, consulte [crear OData v4 extremo mediante ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="882f0-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="882f0-110">Vamos a crear un singleton en el proyecto de API Web utilizando el modelo de datos siguientes:</span><span class="sxs-lookup"><span data-stu-id="882f0-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Modelo de datos](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="882f0-112">Un valor singleton denominado `Umbrella` se definirán según el tipo de `Company`y un conjunto con nombre de entidades `Employees` se definen en función de tipo `Employee`.</span><span class="sxs-lookup"><span data-stu-id="882f0-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="882f0-113">La solución que se usa en este tutorial se puede descargar desde [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="882f0-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="882f0-114">Definir el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="882f0-114">Define the data model</span></span>

1. <span data-ttu-id="882f0-115">Definir los tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="882f0-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="882f0-116">Generar el modelo EDM a partir de los tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="882f0-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="882f0-117">En este caso, `builder.Singleton<Company>("Umbrella")` indica el generador de modelos para crear un valor singleton denominado `Umbrella` en el modelo EDM.</span><span class="sxs-lookup"><span data-stu-id="882f0-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="882f0-118">Los metadatos generados tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="882f0-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="882f0-119">Desde los metadatos podemos ver que la propiedad de navegación `Company` en el `Employees` conjunto de entidades se enlaza a singleton `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="882f0-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="882f0-120">El enlace se realiza automáticamente `ODataConventionModelBuilder`, ya que solo `Umbrella` tiene la `Company` tipo.</span><span class="sxs-lookup"><span data-stu-id="882f0-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="882f0-121">Si no hay ninguna ambigüedad en el modelo, puede usar `HasSingletonBinding` para enlazar explícitamente una propiedad de navegación en un singleton; `HasSingletonBinding` tiene el mismo efecto que usar el `Singleton` atributo en la definición de tipo CLR:</span><span class="sxs-lookup"><span data-stu-id="882f0-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="882f0-122">Definir el controlador de singleton</span><span class="sxs-lookup"><span data-stu-id="882f0-122">Define the singleton controller</span></span>

<span data-ttu-id="882f0-123">Al igual que el controlador de EntitySet, el controlador de singleton hereda de `ODataController`, y debe ser el nombre del controlador singleton `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="882f0-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="882f0-124">Con el fin de controlar tipos diferentes de las solicitudes, acciones tienen que ser predefinidas en el controlador.</span><span class="sxs-lookup"><span data-stu-id="882f0-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="882f0-125">**Atributo enrutamiento** está habilitada de forma predeterminada en WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="882f0-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="882f0-126">Por ejemplo, para definir una acción para controlar consultar `Revenue` de `Company` mediante la ruta de atributo, utilice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="882f0-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="882f0-127">Si no está dispuesto a definir los atributos para cada acción, solo para definir las acciones siguientes [convenciones de enrutamiento de OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="882f0-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="882f0-128">Puesto que una clave no es necesaria para las consultas singleton, las acciones definidas en el controlador de singleton son ligeramente diferentes de las acciones definidas en el controlador de entityset.</span><span class="sxs-lookup"><span data-stu-id="882f0-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="882f0-129">Como referencia, las firmas de método para cada definición de acción en el controlador de singleton se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="882f0-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="882f0-130">Básicamente, esto es todo lo que necesita hacer en el lado del servicio.</span><span class="sxs-lookup"><span data-stu-id="882f0-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="882f0-131">El [proyecto de ejemplo](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene todo el código para la solución y el cliente de OData que muestra cómo utilizar el singleton.</span><span class="sxs-lookup"><span data-stu-id="882f0-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="882f0-132">El cliente se crea siguiendo los pasos descritos en [crear una aplicación de cliente de OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="882f0-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="882f0-133">.</span><span class="sxs-lookup"><span data-stu-id="882f0-133">.</span></span> 

<span data-ttu-id="882f0-134">*Gracias a Leo Hu para el contenido original de este artículo.*</span><span class="sxs-lookup"><span data-stu-id="882f0-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
