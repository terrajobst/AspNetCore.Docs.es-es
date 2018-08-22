---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Novedades de ASP.NET Web API OData 5.3 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: 20ef263ce7cbaef40c16937eaa91d34239fc2ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828485"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="c400a-102">Novedades de ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="c400a-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="c400a-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c400a-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="c400a-104">Este tema describe cuáles son las novedades para ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="c400a-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="c400a-105">Descarga</span><span class="sxs-lookup"><span data-stu-id="c400a-105">Download</span></span>](#download)
- [<span data-ttu-id="c400a-106">Documentación</span><span class="sxs-lookup"><span data-stu-id="c400a-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="c400a-107">Bibliotecas principales de OData</span><span class="sxs-lookup"><span data-stu-id="c400a-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="c400a-108">Nuevas características</span><span class="sxs-lookup"><span data-stu-id="c400a-108">New Features</span></span>](#newf)
- [<span data-ttu-id="c400a-109">Problemas conocidos y cambios importantes</span><span class="sxs-lookup"><span data-stu-id="c400a-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="c400a-110">Correcciones de errores</span><span class="sxs-lookup"><span data-stu-id="c400a-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="c400a-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="c400a-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="c400a-112">Descargar</span><span class="sxs-lookup"><span data-stu-id="c400a-112">Download</span></span>

<span data-ttu-id="c400a-113">Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet.</span><span class="sxs-lookup"><span data-stu-id="c400a-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="c400a-114">Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:</span><span class="sxs-lookup"><span data-stu-id="c400a-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="c400a-115">Documentación</span><span class="sxs-lookup"><span data-stu-id="c400a-115">Documentation</span></span>

<span data-ttu-id="c400a-116">Puede encontrar tutoriales y otra documentación sobre ASP.NET Web API OData en la [sitio web de ASP.NET](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="c400a-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="c400a-117">Bibliotecas principales de OData</span><span class="sxs-lookup"><span data-stu-id="c400a-117">OData Core Libraries</span></span>

<span data-ttu-id="c400a-118">Para OData v4, Web API ahora usa ODataLib versión 6.5.0</span><span class="sxs-lookup"><span data-stu-id="c400a-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="c400a-119">Las nuevas características de ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="c400a-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="c400a-120">Expanda la compatibilidad con $levels en $</span><span class="sxs-lookup"><span data-stu-id="c400a-120">Support for $levels in $expand</span></span>

<span data-ttu-id="c400a-121">Puede usar el $levels consulta opción en $ampliar las consultas.</span><span class="sxs-lookup"><span data-stu-id="c400a-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="c400a-122">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c400a-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="c400a-123">Es equivalente a esta consulta:</span><span class="sxs-lookup"><span data-stu-id="c400a-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="c400a-124">Compatibilidad con tipos de entidad abierto</span><span class="sxs-lookup"><span data-stu-id="c400a-124">Support for Open Entity Types</span></span>

<span data-ttu-id="c400a-125">Un *abrir tipo* es un tipo de stuctured que contiene las propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo.</span><span class="sxs-lookup"><span data-stu-id="c400a-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="c400a-126">Tipos abiertos permiten agregar flexibilidad a los modelos de datos.</span><span class="sxs-lookup"><span data-stu-id="c400a-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="c400a-127">Para obtener más información, consulte xxxx.</span><span class="sxs-lookup"><span data-stu-id="c400a-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="c400a-128">Compatibilidad con las propiedades de colección dinámica de tipos abiertos</span><span class="sxs-lookup"><span data-stu-id="c400a-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="c400a-129">Anteriormente, una propiedad dinámica que tenía que ser un valor único.</span><span class="sxs-lookup"><span data-stu-id="c400a-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="c400a-130">En 5.3, las propiedades dinámicas pueden tener valores de la colección.</span><span class="sxs-lookup"><span data-stu-id="c400a-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="c400a-131">Por ejemplo, en la siguiente carga de JSON, el `Emails` propiedad es una propiedad dinámica y de la colección de tipo de cadena:</span><span class="sxs-lookup"><span data-stu-id="c400a-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="c400a-132">Compatibilidad con la herencia para los tipos complejos</span><span class="sxs-lookup"><span data-stu-id="c400a-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="c400a-133">Ahora los tipos complejos pueden heredar de un tipo base.</span><span class="sxs-lookup"><span data-stu-id="c400a-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="c400a-134">Por ejemplo, un servicio de OData puede definir los siguientes tipos complejos:</span><span class="sxs-lookup"><span data-stu-id="c400a-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="c400a-135">Este es el modelo EDM para este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c400a-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="c400a-136">Para obtener más información, consulte [ejemplos de herencia de tipos complejos de OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="c400a-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="c400a-137">Problemas conocidos y cambios importantes</span><span class="sxs-lookup"><span data-stu-id="c400a-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="c400a-138">Esta sección describen problemas conocidos y cambios importantes en la ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="c400a-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="c400a-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="c400a-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="c400a-140">Opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="c400a-140">Query Options</span></span>

<span data-ttu-id="c400a-141">Problema: $ Anidada expanda con $levels = número máximo de resultados en una profundidad de expansión incorrecto.</span><span class="sxs-lookup"><span data-stu-id="c400a-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="c400a-142">Por ejemplo, dada la siguiente solicitud:</span><span class="sxs-lookup"><span data-stu-id="c400a-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="c400a-143">Si `MaxExpansionDepth` es 5, esta consulta daría lugar a una profundidad de expansión de 6.</span><span class="sxs-lookup"><span data-stu-id="c400a-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="c400a-144">Correcciones de errores y las actualizaciones de características secundarias</span><span class="sxs-lookup"><span data-stu-id="c400a-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="c400a-145">Esta versión también incluye varias correcciones de errores y una característica secundaria actualizaciones.</span><span class="sxs-lookup"><span data-stu-id="c400a-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="c400a-146">Puede encontrar una lista completa aquí:</span><span class="sxs-lookup"><span data-stu-id="c400a-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="c400a-147">Correcciones de errores</span><span class="sxs-lookup"><span data-stu-id="c400a-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="c400a-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="c400a-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="c400a-149">En esta versión hemos realizado un [corrección de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) a algunas de las enumeraciones AllowedFunctions.</span><span class="sxs-lookup"><span data-stu-id="c400a-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="c400a-150">Esta versión no tiene otras correcciones de errores ni nuevas características.</span><span class="sxs-lookup"><span data-stu-id="c400a-150">This release doesn't have any other bug fixes or new features.</span></span>
