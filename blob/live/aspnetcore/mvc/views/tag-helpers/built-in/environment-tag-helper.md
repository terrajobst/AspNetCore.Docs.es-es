---
title: "Aplicación auxiliar de etiqueta de entorno en el núcleo de ASP.NET"
author: pkellner
description: "Aplicación auxiliar de etiquetas de entorno de ASP.NET Core define incluido todas las propiedades"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 32646f1fdaf840f796da1ec573459157a41a86d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="65e6b-103">Aplicación auxiliar de etiqueta de entorno en el núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65e6b-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="65e6b-104">Por [Peter Kellner](http://peterkellner.net) y [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="65e6b-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="65e6b-105">La etiqueta de entorno de aplicación auxiliar representa condicionalmente su contenido entre comillas según el entorno de hospedaje actual.</span><span class="sxs-lookup"><span data-stu-id="65e6b-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="65e6b-106">Su único atributo `names` es una lista separada por comas de entorno de nombres, si cualquier coincide con el entorno actual, se activará el contenido entre comillas que se representará.</span><span class="sxs-lookup"><span data-stu-id="65e6b-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="65e6b-107">Atributos de aplicación auxiliar de etiqueta de entorno</span><span class="sxs-lookup"><span data-stu-id="65e6b-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="65e6b-108">nombres</span><span class="sxs-lookup"><span data-stu-id="65e6b-108">names</span></span>

<span data-ttu-id="65e6b-109">Acepta un solo nombre de entorno de hospedaje o una lista separada por comas de nombres de entorno que desencadenan la representación del contenido adjunto de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="65e6b-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="65e6b-110">Estos valores se comparan con el valor actual devuelto por la propiedad estática de ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="65e6b-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="65e6b-111">Este valor es uno de los siguientes: **ensayo**; **Desarrollo** o **producción**.</span><span class="sxs-lookup"><span data-stu-id="65e6b-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="65e6b-112">La comparación omite los casos.</span><span class="sxs-lookup"><span data-stu-id="65e6b-112">The comparison ignores case.</span></span>

<span data-ttu-id="65e6b-113">Un ejemplo de válido `environment` aplicación auxiliar para etiqueta es:</span><span class="sxs-lookup"><span data-stu-id="65e6b-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="65e6b-114">incluir o excluir los atributos</span><span class="sxs-lookup"><span data-stu-id="65e6b-114">include and exclude attributes</span></span>

<span data-ttu-id="65e6b-115">ASP.NET Core 2.x agrega la `include`  &  `exclude` atributos.</span><span class="sxs-lookup"><span data-stu-id="65e6b-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="65e6b-116">Estos atributos controlan el representar el contenido entre comillas basándose en los nombres de entorno de hospedaje incluidas o excluidas.</span><span class="sxs-lookup"><span data-stu-id="65e6b-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="65e6b-117">incluir principales ASP.NET 2.0 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="65e6b-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="65e6b-118">El `include` propiedad tiene un comportamiento similar de la `names` atributo en ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="65e6b-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="65e6b-119">excluir principales ASP.NET 2.0 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="65e6b-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="65e6b-120">En cambio, el `exclude` propiedad permite el `EnvironmentTagHelper` representar el contenido entre comillas para todos los nombres de entorno de hospedaje excepto el archivo que especificó.</span><span class="sxs-lookup"><span data-stu-id="65e6b-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="65e6b-121">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="65e6b-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
