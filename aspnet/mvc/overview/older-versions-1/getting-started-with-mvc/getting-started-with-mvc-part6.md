---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Agregar un método de creación y crear vista | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 976df78ea22c30c094f70a57792d287f15d2c62d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400913"
---
<a name="adding-a-create-method-and-create-view"></a>Agregar un método de creación y crear vista
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe desde una base de datos. Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección vamos a implementar la compatibilidad necesaria para permitir que los usuarios crear nuevas películas en nuestra base de datos. Haremos esto mediante la implementación de la acción de dirección URL de películas/Create.

Implementación de la dirección URL de películas/Create es un proceso de dos pasos. Cuando un usuario visita la dirección URL de películas y crear por primera vez queremos mostrarles un formulario HTML que se pueden rellenar para especificar una película nueva. A continuación, cuando el usuario envía el formulario y publicaciones de que realizar una copia de los datos en el servidor, desea recuperar el contenido expuesto y guárdelo en nuestra base de datos.

Implementaremos estos dos pasos dentro de dos métodos Create() dentro de nuestra clase nombre moviescontroller al. Un método mostrará el &lt;formulario&gt; que el usuario debe rellenar para crear una película nueva. El segundo método controlará el procesamiento de los datos enviados cuando el usuario envía el &lt;formulario&gt; al servidor y guardar una película nueva dentro de nuestra base de datos.

A continuación es el código que agregaremos a nuestra clase nombre moviescontroller al implementar esto:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

El código anterior contiene todo el código que necesitamos, dentro de nuestro controlador.

Implementemos ahora la plantilla Create View que vamos a usar para mostrar un formulario al usuario. Se le haga clic con el botón derecho en el primer método para crear y seleccione "Agregar vista" para crear la plantilla de vista para nuestro formulario de la película.

Seleccionaremos que se va a pasar a la plantilla de vista de una película"" como su clase de datos de vista e indicar que deseamos "aplicar la técnica scaffolding" una plantilla "Crear".

[![Agregar vista](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Tras hacer clic en el botón Agregar, plantilla de vista \Movies\Create.aspx se crearán automáticamente. Dado que hemos seleccionado "Crear" en la lista desplegable de "ver el contenido", el cuadro de diálogo Agregar vista automáticamente "scaffolding" algún contenido predeterminado para nosotros. La técnica de scaffolding crea un elemento HTML &lt;formulario&gt;, un lugar para error de validación de mensajes para ir y, ya que sabe scaffolding sobre películas, crea campos y etiqueta para cada propiedad de nuestra clase.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Dado que nuestra base de datos automáticamente incluye un identificador de una película, vamos a quitar esos campos ese modelo de referencia. Id. de la vista de creación. Quite las líneas después de 7 &lt;leyenda&gt;campos&lt;/legend&gt; como que muestran el campo de identificador que no queremos.

Ahora vamos a crear una película de nuevo y agregarlo a la base de datos. Se deberá hacer esto mediante la ejecución de la aplicación de nuevo y visite el "/ Movies" dirección URL y haga clic en la sección "crear" vínculo para agregar una nueva película.

[![Crear - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Cuando hacemos clic en el botón Crear, publicaremos nuevo (a través de HTTP POST) los datos en este formulario para el método /Movies/Create que acabamos de crear. Al igual que cuando el sistema automáticamente usó el parámetro "numTimes" y "name" fuera de la dirección URL y ellos asignados a los parámetros en un método anteriormente, el sistema automáticamente toman los campos del formulario de una entrada de blog y asignarlos a un objeto. En este caso, los valores de campos de HTML, como "ReleaseDate" y "Title" se coloca automáticamente en las propiedades correctas de una nueva instancia de una película.

Echemos un vistazo en el segundo método de creación de nuestro nombre moviescontroller al nuevo. Tenga en cuenta cómo toma un objeto de "Movie" como argumento:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Este objeto de película, a continuación, se pasó a la versión [HttpPost] de nuestro método de acción Create, y se guardó en la base de datos y, a continuación, redirige al usuario al método de acción de Index() que mostrará los resultados guardados en la lista de películas:

[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Hemos no estamos comprobando si nuestra películas son correctas, sin embargo, y la base de datos no nos permite guardar una película con ningún título. Sería bueno si podemos decirle al usuario que antes de la base de datos produjo un error. Haremos esto a continuación mediante la adición de compatibilidad de validación a nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part5.md)
> [Siguiente](getting-started-with-mvc-part7.md)
