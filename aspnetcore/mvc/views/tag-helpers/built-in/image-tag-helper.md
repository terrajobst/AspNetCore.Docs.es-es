---
title: Aplicación auxiliar de etiquetas de imagen en ASP.NET Core
author: pkellner
description: Muestra cómo trabajar con la aplicación auxiliar de etiquetas de imagen
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 7ed160354b25aa0183ac49db93307b1f1b4d0666
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276648"
---
# <a name="image-tag-helper-in-aspnet-core"></a>Aplicación auxiliar de etiquetas de imagen en ASP.NET Core

Por [Peter Kellner](http://peterkellner.net) 

La aplicación auxiliar de etiquetas de imagen es una mejora de la etiqueta `img` (`<img>`). Requiere una etiqueta `src`, así como el atributo `boolean` `asp-append-version`.

Si el origen de la imagen (`src`) es un archivo estático en el servidor web del host, se anexa una cadena única de bloqueo de la caché como parámetro de consulta a ese origen de la imagen. De este modo, si el archivo en el servidor web del host cambia, se generará una dirección URL de solicitud única que incluye el parámetro de solicitud actualizada. La cadena de limpieza de memoria caché es un valor único que representa el valor hash del archivo de imagen estático.

Si el origen de la imagen (`src`) no es un archivo estático (por ejemplo, es una dirección URL remota o se trata de un archivo que no existe en el servidor) el atributo `src` de la etiqueta `<img>` se genera sin parámetro de cadena de consulta de limpieza de caché.

## <a name="image-tag-helper-attributes"></a>Atributos de la aplicación auxiliar de etiquetas de imagen


### <a name="asp-append-version"></a>asp-append-version

Cuando se especifica junto con un atributo `src`, se invoca la aplicación auxiliar de etiquetas de imagen.

Este es un ejemplo de aplicación auxiliar de etiquetas `img` válido:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Si el archivo estático existe en el directorio *..wwwroot/images/asplogo.png*, el código HTML generado es similar al siguiente (el valor hash será diferente):

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

El valor asignado al parámetro `v` es el valor hash del archivo almacenado en disco. Si el servidor web no es capaz de obtener acceso de lectura al archivo estático al que se hace referencia, no se agregará ningún parámetro `v` al atributo `src`.

- - -

### <a name="src"></a>src

Para activar la aplicación auxiliar de etiquetas de imagen, se requiere el atributo src en el elemento `<img>`. 

> [!NOTE]
> La aplicación auxiliar de etiquetas de imagen usa el proveedor `Cache` en el servidor web local para almacenar el valor de `Sha512` calculado de un archivo determinado. Si el archivo se vuelve a solicitar, no es necesario volver a calcular `Sha512`. La memoria caché queda invalidada por un monitor del archivo que se adjunta al archivo cuando el valor de `Sha512` del archivo se calcula.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:performance/caching/memory>
