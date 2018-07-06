---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contención en OData v4 usar Web API 2.2 | Microsoft Docs
author: rick-anderson
description: Tradicionalmente, una entidad solo se puede acceder si se encapsule dentro de un conjunto de entidades. Pero OData v4 proporciona dos opciones adicionales, Singleton y Con...
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 56e550b56e9ad237dbf4fab04f2bd545164ee90a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824917"
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="c98ce-104">Usar Web API 2.2 la contención en OData v4</span><span class="sxs-lookup"><span data-stu-id="c98ce-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="c98ce-105">por Tan Jinfu</span><span class="sxs-lookup"><span data-stu-id="c98ce-105">by Jinfu Tan</span></span>

> <span data-ttu-id="c98ce-106">Tradicionalmente, una entidad solo se puede acceder si se encapsule dentro de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="c98ce-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="c98ce-107">Pero OData v4 proporciona dos opciones adicionales, Singleton y contención, ambos de los cuales admite WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="c98ce-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="c98ce-108">En este tema se muestra cómo definir una contención en un extremo de OData en WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="c98ce-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="c98ce-109">Para obtener más información acerca de la contención, vea [contención viene con OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="c98ce-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="c98ce-110">Para crear un punto de conexión de OData V4 en Web API, consulte [crear una mediante ASP.NET Web API 2.2 de OData v4 extremo](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="c98ce-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="c98ce-111">En primer lugar, vamos a crear un modelo de dominio de la contención en el servicio de OData, con este modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="c98ce-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modelo de datos](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="c98ce-113">Una cuenta contiene muchas PaymentInstruments (PI), pero no definimos un conjunto de entidades de una instrucción de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="c98ce-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="c98ce-114">En su lugar, solo se pueden acceder a la PI a través de una cuenta.</span><span class="sxs-lookup"><span data-stu-id="c98ce-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="c98ce-115">Puede descargar la solución emplea en este tema de [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="c98ce-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="c98ce-116">Definir el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="c98ce-116">Defining the data model</span></span>

1. <span data-ttu-id="c98ce-117">Definir los tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="c98ce-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="c98ce-118">El `Contained` atributo se utiliza para las propiedades de navegación de contención.</span><span class="sxs-lookup"><span data-stu-id="c98ce-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="c98ce-119">Generar el modelo EDM a partir de los tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="c98ce-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="c98ce-120">El `ODataConventionModelBuilder` controlará la creación del modelo EDM, si la `Contained` atributo se agrega a la propiedad de navegación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="c98ce-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="c98ce-121">Si la propiedad es un tipo de colección, un `GetCount(string NameContains)` también se creará la función.</span><span class="sxs-lookup"><span data-stu-id="c98ce-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="c98ce-122">Los metadatos generados tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c98ce-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="c98ce-123">El `ContainsTarget` atributo indica que la propiedad de navegación es una contención.</span><span class="sxs-lookup"><span data-stu-id="c98ce-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="c98ce-124">Definir el controlador de conjunto de entidades que contiene</span><span class="sxs-lookup"><span data-stu-id="c98ce-124">Define the containing entity set controller</span></span>

<span data-ttu-id="c98ce-125">Entidades independientes no tienen su propio controlador; la acción se define en el controlador de conjunto de entidades que contiene.</span><span class="sxs-lookup"><span data-stu-id="c98ce-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="c98ce-126">En este ejemplo, hay un AccountsController, pero no PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="c98ce-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="c98ce-127">Si la ruta de acceso de OData es 4 o más segmentos, atributo de sólo funciona el enrutamiento, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` en el controlador anterior.</span><span class="sxs-lookup"><span data-stu-id="c98ce-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="c98ce-128">En caso contrario, atributo y el enrutamiento convencional funciona: por ejemplo, `GetPayInPIs(int key)` coincide con `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="c98ce-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="c98ce-129">*Gracias a Leo Hu para el contenido original de este artículo.*</span><span class="sxs-lookup"><span data-stu-id="c98ce-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
