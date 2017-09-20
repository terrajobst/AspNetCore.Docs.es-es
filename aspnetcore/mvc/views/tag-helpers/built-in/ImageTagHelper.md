---
title: "Aplicación auxiliar de la etiqueta de imagen | Documentos de Microsoft"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiqueta de imagen"
keywords: "ASP.NET Core, aplicación auxiliar de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a013
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/ImageTagHelper
ms.openlocfilehash: e91018be7d706ddc227f82b695a188ed91163f9d
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2017
---
# <a name="imagetaghelper"></a>ImageTagHelper

Por [Peter Kellner](http://peterkellner.net) 

Mejora de la aplicación auxiliar de etiqueta de imagen la `img` (`<img>`) etiqueta. Requiere un `src` etiqueta, así como la `boolean` atributo `asp-append-version`.

Si el origen de imagen (`src`) es un archivo estático en el servidor web de host, se anexa una memoria caché única la desactivación de cadena como parámetro de consulta para el origen de la imagen. Esto garantiza que si cambia el archivo en el servidor web de host, una dirección URL de solicitud único se genera que incluya el parámetro de solicitud actualizada. La memoria caché de la desactivación de cadena es un valor único que representa el valor hash del archivo de imagen estática.

Si el origen de imagen (`src`) no es un archivo estático (por ejemplo una dirección URL remota o en el archivo no existe en el servidor), el `<img>` etiqueta `src` atributo se genera sin caché la desactivación de parámetro de cadena de consulta.

## <a name="image-tag-helper-attributes"></a>Atributos de aplicación auxiliar de etiqueta de imagen


### <a name="asp-append-version"></a>versión anexar ASP

Cuando se especifica junto con un `src` se invoca el atributo, la aplicación auxiliar de etiqueta de imagen.

Un ejemplo de válido `img` aplicación auxiliar para etiqueta es:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Si existe el archivo estático en el directorio *... Wwwroot/images/asplogo.png* el código html generado es similar al siguiente (el valor hash será diferente):

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

El valor asignado al parámetro `v` es el valor hash del archivo en disco. Si el servidor web es no se puede obtener acceso de lectura para el archivo estático al que hace referencia, no `v` parámetros se agregan a la `src` atributo.

- - -

### <a name="src"></a>src

Para activar la aplicación auxiliar de etiqueta de imagen, se requiere el atributo src en la `<img>` elemento. 

> [!NOTE]
> La aplicación auxiliar de etiqueta de imagen utiliza el `Cache` proveedor en el servidor web local para almacenar los calculados `Sha512` de un archivo determinado. Si el archivo se vuelve a solicitar la `Sha512` no es necesario volver a calcular. La memoria caché se invalida por un monitor del archivo que se adjunta al archivo cuando el archivo `Sha512` se calcula.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:performance/caching/memory>
