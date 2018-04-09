---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Diseño y menú categoría | Documentos de Microsoft'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 3 trata la adición de diseño y un menú de categoría.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27a493173b03f813ee3dcbbfafd8bc52fb0b9771
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-3-layout-and-category-menu"></a>Parte 3: Diseño y menú de categoría
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 3 trata la adición de diseño y un menú de categoría.


## <a id="_Toc260221669"></a>  Agregar algunos diseño y un menú de categoría

En la página principal del sitio se agregará un elemento div de la columna del lado izquierdo que contendrá el menú de la categoría de producto.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Tenga en cuenta que la alineación deseada y otras características de formato se proporcionará la clase CSS que hemos agregado a nuestro archivo Style.css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

El menú de la categoría de producto se creará dinámicamente en tiempo de ejecución consultando la base de datos de comercio de vínculos existentes de categorías de productos y la creación de los elementos de menú y el correspondiente.

Para ello, que utilizaremos dos de ASP. Controles de datos eficaces de NET. El control de "Origen de datos de entidad" y "ListView" (control).

![](tailspin-spyworks-part-3/_static/image1.jpg)

Vamos a cambiar a la "Vista de diseño" y usar las aplicaciones auxiliares para configurar los controles.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Vamos a establecer la propiedad de Id. de EntityDataSource en EDS\_categoría\_menú y haga clic en "Configurar el origen de datos".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Seleccione la conexión de CommerceEntities que se creó automáticamente cuando se crea el modelo de origen de datos de entidad para nuestra base de datos de comercio y haga clic en "Siguiente".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Seleccione el nombre del conjunto de entidades de "Categorías" y deje el resto de las opciones como valor predeterminado. Haga clic en "Finalizar".

Ahora vamos a establecer la propiedad de Id. de la instancia del control ListView que se coloca en nuestra página de ListView\_ProductsMenu y activar su aplicación auxiliar.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Aunque podríamos usar opciones de control para formatear la visualización del elemento de datos y formato, la creación de menú sólo requerirá simple marcado por lo que se especificará el código en la vista del origen.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Tenga en cuenta la instrucción "Eval": &lt;% # Eval("CategoryName") %&gt;

La sintaxis ASP.NET &lt;% # %&gt; es una convención abreviada que indica el tiempo de ejecución para ejecutar todo lo que está dentro de y mostrar los resultados "en línea".

La instrucción Eval("CategoryName") indica, para la entrada actual de la colección enlazada de elementos de datos, capturar el valor de los nombres de elemento de modelo de entidad "CatagoryName". Se trata de una sintaxis concisa para una característica muy eficaz.

Permite ejecutar la aplicación.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Tenga en cuenta que ahora se muestra el menú de la categoría de producto cuando se mantiene el mouse sobre uno de los elementos de menú de categoría podemos ver los puntos de vínculo de elemento de menú a una página se ha comenzado aún a implementar denominada ProductsList.aspx y que hemos creado un argumento de cadena de consulta dinámica que contiene la  Id. de categoría.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-2.md)
> [Siguiente](tailspin-spyworks-part-4.md)
