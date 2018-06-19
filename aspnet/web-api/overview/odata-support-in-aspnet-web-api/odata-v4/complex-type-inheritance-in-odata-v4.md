---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herencia de tipo complejo de OData v4 con ASP.NET Web API | Documentos de Microsoft
author: microsoft
description: Según la especificación de OData v4, un tipo complejo puede heredar de otro tipo complejo. (Un tipo complejo es un tipo estructurado sin una clave). API de Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508424"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="ce061-104">Herencia de tipo complejo de OData v4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="ce061-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="ce061-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ce061-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ce061-106">Función de OData v4 [especificación](http://www.odata.org/documentation/odata-version-4-0/), un tipo complejo puede heredar de otro tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="ce061-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="ce061-107">(Un *complejo* es un tipo estructurado sin una clave.) Web API OData 5.3 admite la herencia de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="ce061-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="ce061-108">Este tema muestra cómo crear un entity data model (EDM) con tipos complejos de herencia.</span><span class="sxs-lookup"><span data-stu-id="ce061-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="ce061-109">Para el código fuente completo, vea [ejemplo de herencia de tipo complejo de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="ce061-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ce061-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="ce061-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ce061-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="ce061-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="ce061-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="ce061-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="ce061-113">Jerarquía del modelo</span><span class="sxs-lookup"><span data-stu-id="ce061-113">Model Hierarchy</span></span>

<span data-ttu-id="ce061-114">Para ilustrar la herencia de tipo complejo, vamos a usar la siguiente jerarquía de clases.</span><span class="sxs-lookup"><span data-stu-id="ce061-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="ce061-115">`Shape`es un tipo complejo abstracto.</span><span class="sxs-lookup"><span data-stu-id="ce061-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="ce061-116">`Rectangle`, `Triangle`, y `Circle` se derivan de tipos complejos `Shape`, y `RoundRectangle` deriva de `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="ce061-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="ce061-117">`Window`es un tipo de entidad y contiene un `Shape` instancia.</span><span class="sxs-lookup"><span data-stu-id="ce061-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="ce061-118">Estos son las clases CLR que definen estos tipos.</span><span class="sxs-lookup"><span data-stu-id="ce061-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="ce061-119">Generar el modelo EDM</span><span class="sxs-lookup"><span data-stu-id="ce061-119">Build the EDM Model</span></span>

<span data-ttu-id="ce061-120">Para crear el EDM, puede usar **ODataConventionModelBuilder**, lo que deduce las relaciones de herencia entre los tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="ce061-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="ce061-121">También puede compilar EDM explícitamente, mediante **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="ce061-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="ce061-122">Esto tiene más código, pero ofrece un mayor control sobre el modelo EDM.</span><span class="sxs-lookup"><span data-stu-id="ce061-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="ce061-123">Estos dos ejemplos crea el mismo esquema EDM.</span><span class="sxs-lookup"><span data-stu-id="ce061-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="ce061-124">Documento de metadatos</span><span class="sxs-lookup"><span data-stu-id="ce061-124">Metadata Document</span></span>

<span data-ttu-id="ce061-125">Este es el documento de metadatos de OData, que muestra herencia de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="ce061-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="ce061-126">En el documento de metadatos, puede ver:</span><span class="sxs-lookup"><span data-stu-id="ce061-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="ce061-127">El `Shape` tipo complejo es abstracta.</span><span class="sxs-lookup"><span data-stu-id="ce061-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="ce061-128">El `Rectangle`, `Triangle`, y `Circle` tipo complejo tiene el tipo base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="ce061-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="ce061-129">El `RoundRectangle` tipo tiene el tipo base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="ce061-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="ce061-130">Conversión de tipos complejos</span><span class="sxs-lookup"><span data-stu-id="ce061-130">Casting Complex Types</span></span>

<span data-ttu-id="ce061-131">Ahora se admite la conversión se realiza en tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="ce061-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="ce061-132">Por ejemplo, la siguiente consulta convierte un `Shape` a una `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="ce061-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="ce061-133">Esta es la carga de respuesta:</span><span class="sxs-lookup"><span data-stu-id="ce061-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
