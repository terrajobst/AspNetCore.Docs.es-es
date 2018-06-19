---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: Capa de acceso a datos | Documentos de Microsoft'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 2 describe cómo agregar la capa de acceso a datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890494"
---
<a name="part-2-data-access-layer"></a>Parte 2: Capa de acceso a datos
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 2 describe cómo agregar la capa de acceso a datos.


## <a id="_Toc260221668"></a>  Agregar la capa de acceso a datos

Nuestra aplicación de comercio electrónico dependerán de dos bases de datos.

Para obtener información de cliente, vamos a usar la base de datos estándar de pertenencia a ASP.NET. Para nuestro catálogo de producto y el carro de la compra se implementará una base de datos de SQL Express como se indica a continuación.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Al haber creado la base de datos (Commerce.mdf) en la aplicación de la aplicación\_carpeta de datos se puede empezar a crear la capa de acceso a datos utilizando la entidad de .NET Framework.

Vamos a crear una carpeta denominada "datos\_acceso" y haga clic con el botón secundario en esa carpeta y seleccione "Agregar nuevo elemento".

En la "plantillas instaladas" elemento y, a continuación, seleccione "ADO.NET Entity Data Model" escriba EDM\_Commerce.edmx como el nombre y haga clic en el botón "Agregar".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Elija "Generar desde la base de datos".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Guarde y compile.

Ahora estamos preparados para agregar la primera función – un menú de la categoría de producto.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-1.md)
> [Siguiente](tailspin-spyworks-part-3.md)
