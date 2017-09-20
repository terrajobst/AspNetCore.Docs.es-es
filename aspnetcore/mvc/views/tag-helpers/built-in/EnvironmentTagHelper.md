---
title: "Aplicación auxiliar de etiqueta de entorno en el núcleo de ASP.NET"
author: pkellner
description: "Aplicación auxiliar de etiquetas de entorno de ASP.Net Core define incluido todas las propiedades"
keywords: "ASP.NET Core, aplicación auxiliar de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: e16f942b4a42ef495c7a7daff6724ff071bb35a1
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="9a6b6-104">Aplicación auxiliar de etiqueta de entorno en el núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9a6b6-104">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="9a6b6-105">Por [Peter Kellner](http://peterkellner.net) y [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="9a6b6-105">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="9a6b6-106">La etiqueta de entorno de aplicación auxiliar representa condicionalmente su contenido entre comillas según el entorno de hospedaje actual.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-106">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="9a6b6-107">Su único atributo `names` es una lista separada por comas de entorno de nombres, si cualquier coincide con el entorno actual, se activará el contenido entre comillas que se representará.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-107">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="9a6b6-108">Atributos de aplicación auxiliar de etiqueta de entorno</span><span class="sxs-lookup"><span data-stu-id="9a6b6-108">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="9a6b6-109">nombres</span><span class="sxs-lookup"><span data-stu-id="9a6b6-109">names</span></span>

<span data-ttu-id="9a6b6-110">Acepta un solo nombre de entorno de hospedaje o una lista separada por comas de nombres de entorno que desencadenan la representación del contenido adjunto de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-110">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="9a6b6-111">Estos valores se comparan con el valor actual devuelto por la propiedad estática de ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-111">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="9a6b6-112">Este valor es uno de los siguientes: **ensayo**; **Desarrollo** o **producción**.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-112">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="9a6b6-113">La comparación omite los casos.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-113">The comparison ignores case.</span></span>

<span data-ttu-id="9a6b6-114">Un ejemplo de válido `environment` aplicación auxiliar para etiqueta es:</span><span class="sxs-lookup"><span data-stu-id="9a6b6-114">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="9a6b6-115">incluir o excluir los atributos</span><span class="sxs-lookup"><span data-stu-id="9a6b6-115">include and exclude attributes</span></span>

<span data-ttu-id="9a6b6-116">ASP.NET Core 2.x agrega la `include`  &  `exclude` atributos.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-116">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="9a6b6-117">Estos atributos controlan el representar el contenido entre comillas basándose en los nombres de entorno de hospedaje incluidas o excluidas.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-117">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="9a6b6-118">incluir principales ASP.NET 2.0 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="9a6b6-118">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="9a6b6-119">El `include` propiedad tiene un comportamiento similar de la `names` atributo en ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-119">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="9a6b6-120">excluir principales ASP.NET 2.0 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="9a6b6-120">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="9a6b6-121">En cambio, el `exclude` propiedad permite el `EnvironmentTagHelper` representar el contenido entre comillas para todos los nombres de entorno de hospedaje excepto el archivo que especificó.</span><span class="sxs-lookup"><span data-stu-id="9a6b6-121">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="9a6b6-122">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9a6b6-122">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
