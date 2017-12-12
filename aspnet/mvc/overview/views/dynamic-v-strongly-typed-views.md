---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: "V dinámicos. Establecimiento inflexible de tipos vistas | Documentos de Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a>V dinámicos. Vistas fuertemente tipadas
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

Hay tres maneras para pasar información de un controlador a una vista en ASP.NET MVC 3:

1. Como un objeto de modelo fuertemente tipado.
2. Como un tipo dinámico (con @model dinámica)
3. Usar el elemento ViewBag

He escrito una aplicación de MVC 3 superior Blog simple para comparar y contrastar las vistas dinámicas y fuertemente tipadas. El controlador se inicia con una simple lista de blogs:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Haga clic en el método IndexNotStonglyTyped() y agregar una vista Razor.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Asegúrese de que el **crear una vista fuertemente tipada** casilla no está activada. La vista resultante no contiene gran parte:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Como vamos a usar un dinámico y no una vista fuertemente tipada, intellisense no ayudarnos. El código completo se muestra a continuación:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Ahora vamos a agregar una vista fuertemente tipada. Agregue el código siguiente al controlador:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Tenga en cuenta que es exactamente el View(topBlogs) devuelto mismo; Llame a que la vista no fuertemente tipada. Haga clic con el botón secundario dentro de *StonglyTypedIndex()* y seleccione **agregar vista**. Esta vez, seleccione la **Blog** clase de modelo y seleccione **lista** como la plantilla de scaffolding.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Dentro de la nueva plantilla de vista se obtener compatibilidad con intellisense.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Se puede descargar el proyecto de c# [aquí](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
