---
title: Aplicaciones auxiliares de etiquetas integradas de ASP.NET Core
author: pkellner
description: Aplicaciones auxiliares de etiquetas integradas de ASP.NET Core
keywords: "ASP.NET Core, aplicación auxiliar de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: e7c8c64283ca3740698300689b10497f984cfd3e
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="31856-104">Aplicaciones auxiliares de etiquetas integradas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31856-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="31856-105">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="31856-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="31856-106">ASP.NET Core incluye varias aplicaciones auxiliares de etiquetas integradas para aumentar la productividad.</span><span class="sxs-lookup"><span data-stu-id="31856-106">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="31856-107">En esta sección se proporciona un resumen de las aplicaciones auxiliares de etiquetas integradas.</span><span class="sxs-lookup"><span data-stu-id="31856-107">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="31856-108">Algunas aplicaciones auxiliares de etiquetas integradas no figuran en la sección, ya que se usan internamente en el motor de vistas de [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="31856-108">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="31856-109">Esto incluye una aplicación auxiliar de etiquetas para el carácter ~, que se expande hasta la ruta de acceso raíz del sitio web.</span><span class="sxs-lookup"><span data-stu-id="31856-109">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="31856-110">Aplicaciones auxiliares de etiquetas integradas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31856-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="31856-111">**[Aplicación auxiliar de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="31856-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span></span>

<span data-ttu-id="31856-112">**[Aplicación auxiliar de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="31856-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span></span>

<span data-ttu-id="31856-113">**[Aplicación auxiliar de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="31856-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span></span>

<span data-ttu-id="31856-114">**[Aplicación auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="31856-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/FormActionTagHelper)**

<span data-ttu-id="31856-115">**[Aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="31856-115">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="31856-116">**[Aplicación auxiliar de etiquetas de imagen](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="31856-116">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span></span>

<span data-ttu-id="31856-117">**[Aplicación auxiliar de etiquetas de entrada](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="31856-117">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="31856-118">**[Aplicación auxiliar de etiquetas de elementos de etiqueta](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="31856-118">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/LinkTagHelper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/OptionTagHelper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/ScriptTagTagHelper)**

<span data-ttu-id="31856-119">**[Aplicación auxiliar de etiquetas de selección](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="31856-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="31856-120">**[Aplicación auxiliar de etiquetas de área de texto](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="31856-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="31856-121">**[Aplicación auxiliar de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="31856-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="31856-122">**[Aplicación auxiliar de etiquetas de resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="31856-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31856-123">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="31856-123">Additional resources</span></span>

* [<span data-ttu-id="31856-124">Desarrollo en el cliente</span><span class="sxs-lookup"><span data-stu-id="31856-124">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="31856-125">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="31856-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
