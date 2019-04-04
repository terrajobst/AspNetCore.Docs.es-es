---
title: Asistentes de etiquetas integradas de ASP.NET Core
author: pkellner
description: Descubra cómo los asistentes de etiquetas integradas de ASP.NET Core le ayudan a mejorar su productividad.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 9cca912f43159e778a4c9419e6171f06b4037b8b
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346020"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="54835-103">Asistentes de etiquetas integradas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54835-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="54835-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="54835-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="54835-105">Para obtener información general sobre asistentes de etiquetas, vea <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="54835-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

> [!NOTE]
> <span data-ttu-id="54835-106">Hay asistentes de etiquetas integradas que no se describen en la documentación.</span><span class="sxs-lookup"><span data-stu-id="54835-106">There are built-in Tag Helpers which aren't described in the documentation.</span></span> <span data-ttu-id="54835-107">Estos asistentes de etiquetas se usan internamente por el motor de vista de [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="54835-107">These Tag Helpers are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="54835-108">Esto incluye un asistente de etiquetas para el carácter `~`, que se expande hasta la ruta de acceso raíz del sitio web.</span><span class="sxs-lookup"><span data-stu-id="54835-108">This includes a Tag Helper for the `~` (tilde) character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="54835-109">Asistentes de etiquetas integradas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54835-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="54835-110">**[Asistente de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="54835-111">**[Asistente de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="54835-112">**[Asistente de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="54835-113">**[Asistente de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="54835-114">**[Asistente de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="54835-115">**[Asistente de etiquetas de acción de formulario](xref:mvc/views/working-with-forms#the-form-action-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-115">**[Form Action Tag Helper](xref:mvc/views/working-with-forms#the-form-action-tag-helper)**</span></span>

<span data-ttu-id="54835-116">**[Asistente de etiquetas de imagen](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-116">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="54835-117">**[Asistente de etiquetas de entrada](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-117">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="54835-118">**[Asistente de etiquetas de elementos de etiqueta](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-118">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="54835-119">**[Asistente de etiquetas parciales](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-119">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="54835-120">**[Asistente de etiquetas de selección](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-120">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="54835-121">**[Asistente de etiquetas de área de texto](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-121">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="54835-122">**[Asistente de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-122">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="54835-123">**[Asistente de etiquetas de resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="54835-123">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="54835-124">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="54835-124">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
