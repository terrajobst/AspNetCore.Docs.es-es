---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Mediante la clase TagBuilder para compilar aplicaciones auxiliares HTML (C#) | Documentos de Microsoft
author: StephenWalther
description: Stephen Walther presenta una clase de utilidad en el marco de MVC de ASP.NET con el nombre de la clase TagBuilder. Puede usar la clase TagBuilder fácilmente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c0e8e4e3a733f2cc8690dc85e3006bce6c661d2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870393"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>Mediante la clase TagBuilder para compilar aplicaciones auxiliares HTML (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther presenta una clase de utilidad en el marco de MVC de ASP.NET con el nombre de la clase TagBuilder. Puede utilizar la clase TagBuilder para crear fácilmente etiquetas HTML.


El marco de MVC de ASP.NET incluye una clase de utilidad con el nombre de la clase TagBuilder que puede usar al compilar aplicaciones auxiliares HTML. La clase TagBuilder, tal y como sugiere su nombre de la clase, permite crear fácilmente etiquetas HTML. En este breve tutorial, se proporciona una visión general de la clase TagBuilder y obtenga información acerca de cómo usar esta clase al compilar una aplicación auxiliar HTML simple que representa HTML &lt;img&gt; etiquetas.

## <a name="overview-of-the-tagbuilder-class"></a>Información general de la clase TagBuilder

La clase TagBuilder se encuentra en el espacio de nombres de System.Web.Mvc. Tiene cinco métodos:

- AddCssClass() - le permite agregar un nuevo *clase = ""* a una etiqueta de atributo.
- GenerateId() - le permite agregar un atributo id a una etiqueta. Este método reemplaza automáticamente a los puntos en el Id. (de forma predeterminada, períodos se reemplazan por caracteres de subrayado)
- MergeAttribute() - le permite agregar atributos a una etiqueta. Existen varias sobrecargas de este método.
- SetInnerText() - le permite establecer el texto interno de la etiqueta. El texto interno es codificación HTML automáticamente.
- ToString() - permite presentar la etiqueta. Puede especificar si desea crear una etiqueta normal, una etiqueta inicial, una etiqueta de cierre o una etiqueta de cierre automático.
  

La clase TagBuilder tiene cuatro propiedades importantes:

- Atributos; representa todos los atributos de la etiqueta.
- IdAttributeDotReplacement - representa el carácter utilizado por el método GenerateId() para reemplazar períodos (el valor predeterminado es un carácter de subrayado).
- InnerHTML - representa el contenido interno de la etiqueta. Asigne una cadena a esta propiedad *no* HTML codificar la cadena.
- TagName - representa el nombre de la etiqueta.

Estos métodos y propiedades proporcionan todos los métodos y propiedades básicos que necesita para crear una etiqueta HTML. Realmente no es necesario utilizar la clase TagBuilder. Puede usar una clase StringBuilder en su lugar. Sin embargo, la clase TagBuilder hace que su vida un poco más fácil.

## <a name="creating-an-image-html-helper"></a>Crear una aplicación auxiliar HTML de imagen

Cuando crea una instancia de la clase TagBuilder, pase el nombre de la etiqueta que desea generar al constructor TagBuilder. A continuación, puede llamar a métodos como los métodos AddCssClass y MergeAttribute() para modificar los atributos de la etiqueta. Por último, llame al método ToString() para presentar la etiqueta.

Por ejemplo, el listado 1 contiene una aplicación auxiliar de HTML de la imagen. La aplicación auxiliar de la imagen que se implementa internamente con un TagBuilder que representa un elemento HTML &lt;img&gt; etiqueta.

**Lista 1 - Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

La clase en el listado 1 contiene dos métodos sobrecargados estáticos con el nombre de imagen. Cuando se llama al método Image(), puede pasar un objeto que representa un conjunto de atributos HTML o no.

Tenga en cuenta cómo se utiliza el método TagBuilder.MergeAttribute() para agregar los atributos individuales, como el atributo src a la TagBuilder. Además, tenga en cuenta cómo se utiliza el método TagBuilder.MergeAttributes() para agregar una colección de atributos para el TagBuilder. El método MergeAttributes() acepta un diccionario&lt;cadena, objeto&gt; parámetro. La clase RouteValueDictionary se utiliza para convertir el objeto que representa la colección de atributos en un diccionario&lt;cadena, objeto&gt;.

Después de crear la aplicación auxiliar de la imagen, puede utilizar la aplicación auxiliar en las vistas de MVC de ASP.NET como cualquiera de los otros Ayudantes HTML estándares. La vista en la lista 2 usa la aplicación auxiliar de imagen para mostrar la misma imagen de una consola Xbox dos veces (consulte la figura 1). Se llama a la aplicación auxiliar Image() con y sin una colección de atributos HTML.

**La lista 2 - Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![El cuadro de diálogo nuevo proyecto](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**Figura 01**: utilizar la aplicación auxiliar de imagen ([haga clic aquí para ver la imagen a tamaño completo](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


Tenga en cuenta que se debe importar el espacio de nombres asociado a la aplicación auxiliar de imagen en la parte superior de la vista Index.aspx. La aplicación auxiliar se importa con la siguiente directiva:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [Anterior](creating-custom-html-helpers-cs.md)
> [Siguiente](creating-page-layouts-with-view-master-pages-cs.md)
