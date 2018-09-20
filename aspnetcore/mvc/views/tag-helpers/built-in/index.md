---
title: Asistentes de etiquetas integradas de ASP.NET Core
author: pkellner
description: Descubra cómo los asistentes de etiquetas integradas de ASP.NET Core le ayudan a mejorar su productividad.
ms.author: riande
ms.date: 09/18/2018
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 5d9425e0b68578c86a6f9a691322e0bb63a860fb
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292315"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="3adcc-103">Asistentes de etiquetas integradas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3adcc-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="3adcc-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="3adcc-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="3adcc-105">ASP.NET Core incluye varios asistentes de etiquetas integradas para aumentar la productividad.</span><span class="sxs-lookup"><span data-stu-id="3adcc-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="3adcc-106">En esta sección se proporciona un resumen de los asistentes de etiquetas integradas.</span><span class="sxs-lookup"><span data-stu-id="3adcc-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="3adcc-107">Algunos asistentes de etiquetas integradas no figuran en la sección, ya que se usan internamente en el motor de vistas de [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="3adcc-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="3adcc-108">Esto incluye un asistente de etiquetas para el carácter ~, que se expande hasta la ruta de acceso raíz del sitio web.</span><span class="sxs-lookup"><span data-stu-id="3adcc-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="3adcc-109">Asistentes de etiquetas integradas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3adcc-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="3adcc-110">**[Asistente de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="3adcc-111">**[Asistente de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="3adcc-112">**[Asistente de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="3adcc-113">**[Asistente de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="3adcc-114">**[Asistente de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="3adcc-115">**[Asistente de etiquetas de imagen](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="3adcc-116">**[Asistente de etiquetas de entrada](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="3adcc-117">**[Asistente de etiquetas de elementos de etiqueta](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="3adcc-118">**[Asistente de etiquetas parciales](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="3adcc-119">**[Asistente de etiquetas de selección](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="3adcc-120">**[Asistente de etiquetas de área de texto](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="3adcc-121">**[Asistente de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="3adcc-122">**[Asistente de etiquetas de resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3adcc-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3adcc-123">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3adcc-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
