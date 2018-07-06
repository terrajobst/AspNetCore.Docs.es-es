---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: Capa de acceso a datos | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 2 describe cómo agregar la capa de acceso a datos.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: c1d61681952cbf1f0587f0e42da21f8436d79ebb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812037"
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
