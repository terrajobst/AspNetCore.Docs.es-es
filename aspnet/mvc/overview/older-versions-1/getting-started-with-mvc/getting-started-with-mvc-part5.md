---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Acceso a los datos del modelo desde un controlador | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 556eed77cbd9e81c0a2a1334bb0a8ee56abafd34
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815013"
---
<a name="accessing-your-models-data-from-a-controller"></a>Acceso a los datos del modelo desde un controlador
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe desde una base de datos. Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección vamos a crear una nueva clase de nombre moviescontroller al y escribir algún código que recupera los datos de película y lo muestra al explorador mediante una plantilla de vista.

Haga clic con el botón derecho en la carpeta Controllers y realice un nombre moviescontroller al nuevo.

[![Agregar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Esto creará un nuevo archivo "MoviesController.cs" bajo la carpeta \Controllers dentro de nuestro proyecto. Vamos a actualizar el MovieController para recuperar la lista de películas de nuestra base de datos recién agregado.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Se está realizando una consulta LINQ para que solo recuperamos películas publicadas después el verano de 1984. Necesitaremos una plantilla de vista para representar esta lista de películas de vuelta, así que haga clic en el método y seleccione Agregar vista para crearlo.

En el cuadro de diálogo Agregar vista indicaremos que pasamos una lista&lt;Movies.Models.Movie&gt; a nuestra plantilla de vista. A diferencia de las horas anteriores se usa el cuadro de diálogo Agregar vista y decidió crear una plantilla "vacía", esta vez que indicaremos que queremos que Visual Studio para automáticamente "aplicar scaffolding a" una plantilla de vista para que podamos con algún contenido de forma predeterminada. Haremos esto seleccionando el elemento "List" en el menú"ver contenido de lista desplegable.

Recuerde que cuando ha creado una nueva clase, deberá compilar la aplicación para que se muestre en el cuadro de diálogo Agregar vista.

![Agregar vista](getting-started-with-mvc-part5/_static/image3.png)

Haga clic en Agregar y el sistema generará automáticamente el código para obtener una vista para que podamos que muestra nuestra lista de películas. Esto es un buen momento para cambiar la &lt;h2&gt; encabezado a algo parecido a "My Movie List", como hicimos anteriormente con la vista de Hello World.

[![Películas - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Ejecute la aplicación y visite /Movies en la barra de direcciones. Ahora hemos recupera datos de la base de datos mediante una consulta básica en el controlador y devuelve los datos a una vista que sabe acerca de las películas. Esa vista, a continuación, pone a través de la lista de películas y crea una tabla de datos para nosotros.

[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Se no implementar funcionalidad de edición, detalles y eliminación con esta aplicación, por lo que no necesitamos los vínculos predeterminados creados por la plantilla scaffold para nosotros. Abra el archivo /Movies/Index.aspx y quitarlas.

Este es el código fuente para el aspecto que debería nuestra plantilla de vista actualizada una vez que realizamos estos cambios:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Crear vínculos que no necesitamos, por lo que deberá eliminarlos en este ejemplo. Mantendremos el vínculo Crear nuevo sin embargo, ya que es el siguiente. Este es el aspecto de nuestra aplicación con esa columna eliminada.

[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Ahora tenemos una lista simple de nuestros datos de la película. Sin embargo, si hacemos clic en el vínculo "Crear nuevo", obtendremos un error que no se enlazó! Vamos a implementar un método de acción Create y que los usuarios puedan introducir nuevas películas en nuestra base de datos.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part4.md)
> [Siguiente](getting-started-with-mvc-part6.md)
