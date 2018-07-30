---
title: Aplicación auxiliar de etiquetas de entorno en ASP.NET Core
author: pkellner
description: Aplicación auxiliar de etiquetas de entorno de ASP.NET Core definida con todas las propiedades
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342229"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="d6619-103">Aplicación auxiliar de etiquetas de entorno en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6619-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="d6619-104">Por [Peter Kellner](http://peterkellner.net) y [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="d6619-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="d6619-105">La aplicación auxiliar de etiquetas de entorno representa condicionalmente el contenido incluido en función del entorno de hospedaje actual.</span><span class="sxs-lookup"><span data-stu-id="d6619-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="d6619-106">Su único atributo `names` es una lista separada por comas de nombres de entorno que, en caso de que alguno coincida con el entorno actual, hará que se represente el contenido incluido.</span><span class="sxs-lookup"><span data-stu-id="d6619-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="d6619-107">Atributos de aplicación auxiliar de etiquetas de entorno</span><span class="sxs-lookup"><span data-stu-id="d6619-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="d6619-108">nombres</span><span class="sxs-lookup"><span data-stu-id="d6619-108">names</span></span>

<span data-ttu-id="d6619-109">Acepta un solo nombre de entorno de hospedaje o una lista separada por comas de nombres de entorno de hospedaje que desencadenan la representación del contenido incluido.</span><span class="sxs-lookup"><span data-stu-id="d6619-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="d6619-110">Estos valores se comparan con el valor actual devuelto desde la propiedad estática `HostingEnvironment.EnvironmentName` de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6619-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="d6619-111">Este valor es uno de los siguientes: **Staging**, **Development** o **Production**.</span><span class="sxs-lookup"><span data-stu-id="d6619-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="d6619-112">La comparación ignora el uso de mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d6619-112">The comparison ignores case.</span></span>

<span data-ttu-id="d6619-113">Un ejemplo de una aplicación auxiliar de etiquetas `environment` válida es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="d6619-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="d6619-114">Atributos include y exclude</span><span class="sxs-lookup"><span data-stu-id="d6619-114">include and exclude attributes</span></span>

<span data-ttu-id="d6619-115">ASP.NET Core 2.x agrega los atributos `include` & `exclude`.</span><span class="sxs-lookup"><span data-stu-id="d6619-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="d6619-116">Estos atributos controlan la representación del contenido incluido en función de los nombres de entorno de hospedaje incluidos o excluidos.</span><span class="sxs-lookup"><span data-stu-id="d6619-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="d6619-117">Propiedad include de ASP.NET 2.0 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="d6619-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="d6619-118">La propiedad `include` tiene un comportamiento similar al del atributo `names` en ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="d6619-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="d6619-119">Propiedad exclude de ASP.NET Core 2.0 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="d6619-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="d6619-120">En cambio, la propiedad `exclude` permite que `EnvironmentTagHelper` represente el contenido incluido para todos los nombres de entorno de hospedaje, excepto los que haya especificado.</span><span class="sxs-lookup"><span data-stu-id="d6619-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="d6619-121">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d6619-121">Additional resources</span></span>

* <xref:fundamentals/environments>
