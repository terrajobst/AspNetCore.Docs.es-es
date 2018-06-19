---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Obtiene acceso a datos de su modelo desde un controlador | Documentos de Microsoft
author: shanselman
description: Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Crear una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
ms.locfileid: "30876441"
---
<a name="accessing-your-models-data-from-a-controller"></a>Obtiene acceso a datos de su modelo desde un controlador
====================
by [Scott Hanselman](https://github.com/shanselman)

> Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Se creará una aplicación web simple que lee y escribe desde una base de datos. Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección, vamos a crear una nueva clase MoviesController y escribir un cierto código que recupera los datos de la película y muestra el explorador utilizando una plantilla de vista.

Haga clic con el botón secundario en la carpeta Controllers y realice un nuevo MoviesController.

[![Agregar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Esto creará un nuevo archivo de "MoviesController.cs" bajo la carpeta \Controllers dentro de nuestro proyecto. Vamos a actualizar el MovieController para recuperar la lista de películas de nuestra base de datos recién agregado.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Se está realizando una consulta LINQ para que solo recuperamos películas publicadas después del verano de 1984. Necesitaremos una plantilla de vista para representar esta lista de películas nuevo, por lo que haga en el método y seleccione Agregar vista para crearla.

En el cuadro de diálogo Agregar vista le indicamos que pasamos una lista&lt;Movies.Models.Movie&gt; a la plantilla de vista. A diferencia de las horas anteriores, se usa el cuadro de diálogo Agregar vista y decide crear una plantilla de "Vacía", esta vez, le indicamos que queremos Visual Studio para automáticamente "aplicar la técnica scaffolding" una plantilla de vista para que podamos con algún contenido de forma predeterminada. Deberá hacerlo seleccionando el elemento "List" en el menú"ver contenido de lista desplegable.

Recuerde que, cuando se ha creado una nueva clase deberá compilar la aplicación para que se muestre en el cuadro de diálogo Agregar vista.

![Agregar vista](getting-started-with-mvc-part5/_static/image3.png)

Haga clic en Agregar y el sistema generará automáticamente el código para obtener una vista para que podamos que muestra la lista de películas. Esto es un buen momento para cambiar la &lt;h2&gt; encabezado en algo similar a "Mi lista de películas" como hicimos anteriormente con la vista de Hello World.

[![Películas - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Ejecute la aplicación y visite /Movies en la barra de direcciones. Ahora hemos recuperar datos de la base de datos mediante una consulta básica en el controlador y devuelve los datos a una vista que conoce películas. A continuación, esa vista gira a través de la lista de películas y crea una tabla de datos para que podamos.

[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Se no puede implementar funcionalidad de edición, detalles y Delete con esta aplicación, por tanto, no necesitamos los vínculos predeterminados creados por la plantilla de scaffolding para nosotros. Abra el archivo /Movies/Index.aspx y quitarlas.

Este es el código de origen para el aspecto que debería nuestra plantilla vista actualizada una vez que se realice estos cambios:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Está creando vínculos que no necesitamos, por lo que deberá eliminarlos en este ejemplo. Le mantendremos nuestro vínculo Crear nuevo sin embargo, puesto que éste es el siguiente. Este es el aspecto de nuestra aplicación con esa columna eliminada.

[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Ahora tenemos una lista simple de los datos de la película. No obstante, si hacemos clic en el vínculo "Crear nuevo", se obtendrá un error como no se enlazó! Vamos a implementar un método de acción de crear y permitir a los usuarios escribir nuevas películas en nuestra base de datos.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part4.md)
> [Siguiente](getting-started-with-mvc-part6.md)
