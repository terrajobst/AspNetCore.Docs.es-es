---
title: Aplicaciones auxiliares de etiquetas integradas de ASP.NET Core
author: pkellner
description: Aplicaciones auxiliares de etiquetas integradas de ASP.NET Core
manager: wpickett
ms.author: riande
ms.date: 09/13/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 1d086bc0bc210fe19dda34d13008a49d48801607
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="b8096-103">Aplicaciones auxiliares de etiquetas integradas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8096-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="b8096-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="b8096-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="b8096-105">ASP.NET Core incluye varias aplicaciones auxiliares de etiquetas integradas para aumentar la productividad.</span><span class="sxs-lookup"><span data-stu-id="b8096-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="b8096-106">En esta sección se proporciona un resumen de las aplicaciones auxiliares de etiquetas integradas.</span><span class="sxs-lookup"><span data-stu-id="b8096-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="b8096-107">Algunas aplicaciones auxiliares de etiquetas integradas no figuran en la sección, ya que se usan internamente en el motor de vistas de [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="b8096-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="b8096-108">Esto incluye una aplicación auxiliar de etiquetas para el carácter ~, que se expande hasta la ruta de acceso raíz del sitio web.</span><span class="sxs-lookup"><span data-stu-id="b8096-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="b8096-109">Aplicaciones auxiliares de etiquetas integradas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8096-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="b8096-110">**[Aplicación auxiliar de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="b8096-111">**[Aplicación auxiliar de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="b8096-112">**[Aplicación auxiliar de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="b8096-113">**[Aplicación auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="b8096-114">**[Aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="b8096-115">**[Aplicación auxiliar de etiquetas de imagen](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="b8096-116">**[Aplicación auxiliar de etiquetas de entrada](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="b8096-117">**[Aplicación auxiliar de etiquetas de elementos de etiqueta](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="b8096-118">**[Aplicación auxiliar de etiquetas parciales](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="b8096-119">**[Aplicación auxiliar de etiquetas de selección](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="b8096-120">**[Aplicación auxiliar de etiquetas de área de texto](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="b8096-121">**[Aplicación auxiliar de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="b8096-122">**[Aplicación auxiliar de etiquetas de resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b8096-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b8096-123">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b8096-123">Additional resources</span></span>

* [<span data-ttu-id="b8096-124">Desarrollo del lado cliente</span><span class="sxs-lookup"><span data-stu-id="b8096-124">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="b8096-125">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="b8096-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
