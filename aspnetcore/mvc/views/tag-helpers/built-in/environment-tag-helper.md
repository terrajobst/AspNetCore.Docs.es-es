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
# <a name="environment-tag-helper-in-aspnet-core"></a>Aplicación auxiliar de etiqueta de entorno en el núcleo de ASP.NET

Por [Peter Kellner](http://peterkellner.net) y [Ateya Hisham Bin](https://twitter.com/hishambinateya)

La etiqueta de entorno de aplicación auxiliar representa condicionalmente su contenido entre comillas según el entorno de hospedaje actual. Su único atributo `names` es una lista separada por comas de entorno de nombres, si cualquier coincide con el entorno actual, se activará el contenido entre comillas que se representará.

## <a name="environment-tag-helper-attributes"></a>Atributos de aplicación auxiliar de etiqueta de entorno

### <a name="names"></a>nombres

Acepta un solo nombre de entorno de hospedaje o una lista separada por comas de nombres de entorno que desencadenan la representación del contenido adjunto de hospedaje.

Estos valores se comparan con el valor actual devuelto por la propiedad estática de ASP.NET Core `HostingEnvironment.EnvironmentName`.  Este valor es uno de los siguientes: **ensayo**; **Desarrollo** o **producción**. La comparación omite los casos.

Un ejemplo de válido `environment` aplicación auxiliar para etiqueta es:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>incluir o excluir los atributos

ASP.NET Core 2.x agrega la `include`  &  `exclude` atributos. Estos atributos controlan el representar el contenido entre comillas basándose en los nombres de entorno de hospedaje incluidas o excluidas.

### <a name="include-aspnet-core-20-and-later"></a>incluir principales ASP.NET 2.0 y versiones posteriores

El `include` propiedad tiene un comportamiento similar de la `names` atributo en ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>excluir principales ASP.NET 2.0 y versiones posteriores

En cambio, el `exclude` propiedad permite el `EnvironmentTagHelper` representar el contenido entre comillas para todos los nombres de entorno de hospedaje excepto el archivo que especificó.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
