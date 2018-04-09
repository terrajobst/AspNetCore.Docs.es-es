---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Agregar un método de creación y crear vista | Documentos de Microsoft
author: shanselman
description: Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Crear una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 48e656a0c394b9db5baaec9c557ec38c4020d41b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-create-method-and-create-view"></a>Agregar un método de creación y crear vista
====================
por [Scott Hanselman](https://github.com/shanselman)

> Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Se creará una aplicación web simple que lee y escribe desde una base de datos. Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección, vamos a implementar la compatibilidad necesaria para permitir a los usuarios crear nuevas películas en nuestra base de datos. Deberá hacerlo mediante la implementación de la acción de dirección URL de películas/Create.

La implementación de la dirección URL de películas/crear es un proceso de dos pasos. Cuando un usuario visita la dirección URL de películas/crear por primera vez que deseamos mostrar a un formulario HTML que puede rellenar para escribir una película nuevo. A continuación, cuando el usuario envía el formulario y publicaciones de que realizar copias de los datos al servidor, desea recuperar el contenido expuesto y guárdelo en nuestra base de datos.

Implementaremos estos dos pasos en dos métodos de Create() dentro de nuestra clase MoviesController. Un método mostrará el &lt;formulario&gt; que el usuario debe rellenar para crear una película nuevo. El segundo método controlará el procesamiento de los datos enviados cuando el usuario envía la &lt;formulario&gt; al servidor y guardar una película nuevo dentro de nuestra base de datos.

A continuación se muestra el código vamos a agregar a nuestra clase MoviesController implementar esto:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

El código anterior contiene todo el código que necesitaremos dentro de nuestro controlador.

Implementemos ahora la plantilla Create View que vamos a usar para mostrar un formulario para el usuario. Se le haga clic en el primer método de crear y seleccione "Agregar vista" para crear la plantilla de vista de nuestro formulario de película.

Se seleccionará que se va a pasar a la plantilla de vista de una "película" como su clase de vista de datos e indican que deseamos "aplicar la técnica scaffolding" una plantilla "Crear".

[![Agregar vista](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Tras hacer clic en el botón Agregar, plantilla de vista de \Movies\Create.aspx se creará para usted. Dado que hemos seleccionado "Crear" en la lista desplegable de "vista contenido", el cuadro de diálogo Agregar vista automáticamente "scaffolding" algún contenido predeterminado para nosotros. La técnica scaffolding creado HTML &lt;formulario&gt;, un lugar para error de validación de mensajes para ir y, puesto que la técnica scaffolding sabe de películas, crea etiqueta y los campos para cada propiedad de la clase.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Puesto que nuestra base de datos proporciona automáticamente una película en un identificador, vamos a quitar esos campos ese modelo de referencia. Id. de la vista de creación. Elimine las líneas después de 7 &lt;leyenda&gt;campos&lt;/legend&gt; tal y como se muestran el campo de identificador que no es aconsejable.

Ahora vamos a crear una película nuevo y lo agrega a la base de datos. Se deberá hacer esto mediante la ejecución de la aplicación de nuevo y visite el "/ películas" dirección URL y haga clic en "Crear" Use el vínculo para agregar una película nuevo.

[![Crear - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Cuando se hace clic en el botón Crear, que publicaremos nuevo (a través de HTTP POST) los datos en este formulario para el método /Movies/Create que acaba de crear. Al igual que cuando el sistema automáticamente tardó el parámetro "numTimes" y "nombre" fuera de la dirección URL y ellos asignados a los parámetros en un método anteriormente, el sistema automáticamente toman los campos de formulario de una entrada de blog y asignarlos a un objeto. En este caso, los valores de los campos en formato HTML como "ReleaseDate" y "Title" automáticamente se colocan en las propiedades correctas de una nueva instancia de una película.

Echemos un vistazo al segundo método Create de nuestro MoviesController nuevo. Tenga en cuenta cómo toma un objeto de "Película" como argumento:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Este objeto de película, a continuación, se pasó a la versión [HttpPost] de nuestro método de acción Create, y se guardan en la base de datos y, a continuación, redirige al usuario al método de acción de Index() que mostrará el resultado guardado en la lista de película:

[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Hemos no estamos comprobando si nuestro películas son correctos, sin embargo, y la base de datos no permite guardar una película con ningún título. Sería bueno si podríamos indicamos al usuario que antes de la base de datos produjo un error. Esto a continuación, haremos agregando compatibilidad de validación a la aplicación.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part5.md)
> [Siguiente](getting-started-with-mvc-part7.md)
