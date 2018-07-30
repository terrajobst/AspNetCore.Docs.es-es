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
# <a name="environment-tag-helper-in-aspnet-core"></a>Aplicación auxiliar de etiquetas de entorno en ASP.NET Core

Por [Peter Kellner](http://peterkellner.net) y [Hisham Bin Ateya](https://twitter.com/hishambinateya)

La aplicación auxiliar de etiquetas de entorno representa condicionalmente el contenido incluido en función del entorno de hospedaje actual. Su único atributo `names` es una lista separada por comas de nombres de entorno que, en caso de que alguno coincida con el entorno actual, hará que se represente el contenido incluido.

## <a name="environment-tag-helper-attributes"></a>Atributos de aplicación auxiliar de etiquetas de entorno

### <a name="names"></a>nombres

Acepta un solo nombre de entorno de hospedaje o una lista separada por comas de nombres de entorno de hospedaje que desencadenan la representación del contenido incluido.

Estos valores se comparan con el valor actual devuelto desde la propiedad estática `HostingEnvironment.EnvironmentName` de ASP.NET Core.  Este valor es uno de los siguientes: **Staging**, **Development** o **Production**. La comparación ignora el uso de mayúsculas y minúsculas.

Un ejemplo de una aplicación auxiliar de etiquetas `environment` válida es el siguiente:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>Atributos include y exclude

ASP.NET Core 2.x agrega los atributos `include` & `exclude`. Estos atributos controlan la representación del contenido incluido en función de los nombres de entorno de hospedaje incluidos o excluidos.

### <a name="include-aspnet-core-20-and-later"></a>Propiedad include de ASP.NET 2.0 y versiones posteriores

La propiedad `include` tiene un comportamiento similar al del atributo `names` en ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>Propiedad exclude de ASP.NET Core 2.0 y versiones posteriores

En cambio, la propiedad `exclude` permite que `EnvironmentTagHelper` represente el contenido incluido para todos los nombres de entorno de hospedaje, excepto los que haya especificado.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/environments>
