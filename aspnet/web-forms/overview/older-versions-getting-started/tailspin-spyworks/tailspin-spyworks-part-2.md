---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: Capa de acceso a datos | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 2 describe cómo agregar la capa de acceso a datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 13abb02e76b3af80aa11d09e75dc223403917804
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382574"
---
<a name="part-2-data-access-layer"></a>Parte 2: Capa de acceso a datos
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 2 describe cómo agregar la capa de acceso a datos.


## <a id="_Toc260221668"></a>  Adición de la capa de acceso a datos

Nuestra aplicación de comercio electrónico dependerán de dos bases de datos.

Para obtener información de cliente, vamos a usar la base de datos de pertenencia ASP.NET estándar. Para nuestro catálogo de carro y producto compras implementaremos una base de datos de SQL Express como sigue.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Haber creado la base de datos (Commerce.mdf) en la aplicación de la aplicación\_carpeta de datos, podemos continuar para crear la capa de acceso a datos mediante Entity Framework. NET.

Vamos a crear una carpeta denominada "datos\_acceso" y haga clic con el botón derecho en esa carpeta y seleccione "Agregar nuevo elemento".

En "Installed Templates" elemento y, a continuación, seleccione "ADO.NET Entity Data Model" escriba EDM\_Commerce.edmx como el nombre y haga clic en el botón "Agregar".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Elija "Generar desde la base de datos".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Guarde y compile.

Ahora estamos listos para agregar la primera función: un menú de la categoría de producto.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-1.md)
> [Siguiente](tailspin-spyworks-part-3.md)
