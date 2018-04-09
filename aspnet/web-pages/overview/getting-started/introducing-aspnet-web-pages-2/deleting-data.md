---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: 'Introducción a ASP.NET Web Pages: se eliminarán los datos de la base de datos | Documentos de Microsoft'
author: tfitzmac
description: Este tutorial muestra cómo eliminar una entrada de la base de datos individuales. Supone que ha completado la serie a través de actualizar los datos de base de datos de ASP.NET Web PA...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 146199e862cd6fa2607671d31633476b1cb67021
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Introducción a las páginas Web ASP.NET: se eliminarán los datos de la base de datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo eliminar una entrada de la base de datos individuales. Supone que ha completado la serie a través de [actualizar la base de datos en ASP.NET Web Pages](updating-data.md).
> 
> Lo que aprenderá:
> 
> - Cómo seleccionar un registro individual de una lista de registros.
> - Cómo eliminar un único registro de una base de datos.
> - Cómo comprobar que se ha hecho clic en un botón específico en un formulario.
>   
> 
> Características y tecnologías que se tratan:
> 
> - El `WebGrid` auxiliar.
> - La instrucción SQL `Delete` comando.
> - El `Database.Execute` método para ejecutar una instancia de SQL `Delete` comando.


## <a name="what-youll-build"></a>Lo que vamos a compilar

En el tutorial anterior, aprendió a actualizar un registro de base de datos existente. Este tutorial es similar, salvo que en lugar de actualizar el registro, podrá eliminarla. Los procesos son igual, salvo que eliminar es más sencillo, por lo que este tutorial son corto.

En el *películas* página, podrá actualizar la `WebGrid` auxiliar por lo que muestra un **eliminar** vínculo situado junto a cada película para acompañar la **editar** vínculo que agregó anteriormente.

![Página de películas que muestra un vínculo de eliminación para cada película](deleting-data/_static/image1.png)

Al igual que con la edición, al hacer clic en el **eliminar** vínculo, le lleva a una página distinta, donde la información de películas ya está en un formulario:

![Eliminar página de la película con una película muestra](deleting-data/_static/image2.png)

A continuación, hacer clic en el botón para eliminar el registro de forma permanente.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Agregar un vínculo Eliminar a la lista de películas

Podrá empezar agregando una **eliminar** vincular a la `WebGrid` auxiliar. Este vínculo es similar a la **editar** vínculo que agregó en el tutorial anterior.

Abra la *Movies.cshtml* archivo.

Cambiar el `WebGrid` marcado en el cuerpo de la página mediante la adición de una columna. Este es el marcado modificado:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Esta es la nueva columna:

[!code-html[Main](deleting-data/samples/sample2.html)]

La manera en que se configura la cuadrícula, la **editar** columna está en la cuadrícula de la parte izquierda y la **eliminar** columna es más a la derecha. (Hay una coma después de la `Year` columna ahora, en caso de no tenga en cuenta que.) No hay nada especial acerca de dónde ir estas columnas de vínculo y tan fácilmente podría poner junto a la otra. En este caso, son independientes para hacerlos más difíciles se mezclen.

![Página de películas con vínculos de edición y detalles de la marca para indicar que no encuentra al lado de otra](deleting-data/_static/image3.png)

La nueva columna muestra un vínculo (`<a>` elemento) cuyo texto dice "Delete". El destino del vínculo (su `href` atributo) es el código que finalmente se resuelve como algo parecido a esta dirección URL, con el `id` valor diferente para cada película:

[!code-css[Main](deleting-data/samples/sample3.css)]

Este vínculo va a invocar una página denominada *DeleteMovie* y pase el Id. de la película que ha seleccionado.

Este tutorial no incluyen detalles sobre cómo se crea este vínculo, porque es casi idéntico a la **editar** vínculo desde el tutorial anterior ([actualizar la base de datos en ASP.NET Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Creación de la página de borrado

Ahora puede crear la página que será el destino para la **eliminar** vínculo en la cuadrícula.

> [!NOTE] 
> 
> **Importante** la técnica de seleccionando primero un registro para eliminar y, a continuación, con una página independiente y un botón para confirmar el proceso es muy importante para la seguridad. Tal y como se ha leído en tutoriales anteriores, realizar *cualquier* tipo de cambio en el sitio Web debe *siempre* realizarse mediante un formulario &mdash; es decir, usando una operación HTTP POST. Si ha realizado posible cambiar el sitio simplemente haciendo clic en un vínculo (es decir, con una operación GET), personas pudieron hacer solicitudes sencillas a su sitio y eliminar los datos. Incluso un agente del motor de búsqueda que se está indizando el sitio podría eliminar accidentalmente datos por los vínculos siguientes.
> 
> Cuando la aplicación permite que personas cambia un registro, tendrá que presenta el registro al usuario para su edición de todos modos. Pero podría verse tentado a omitir este paso para eliminar un registro. No omita este paso, aunque. (También es útil para los usuarios vean el registro y confirme que está eliminando el registro que han diseñado).
> 
> En un conjunto de tutoriales posterior, verá cómo agregar funcionalidad de inicio de sesión para que un usuario tendría que iniciar sesión antes de eliminar un registro.


Crear una página denominada *DeleteMovie.cshtml* y sustituir lo que aparece en el archivo con el siguiente marcado:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Este marcado es similar a la *EditMovie* páginas, salvo que en lugar de usar los cuadros de texto (`<input type="text">`), incluye el marcado `<span>` elementos. No hay nada aquí para editar. Todo lo que tiene que hacer es mostrar los detalles de la película para que los usuarios pueden asegurarse de que está eliminando la película derecho.

El marcado ya contiene un vínculo que permite al usuario volver a la página de lista de películas.

Como en el *EditMovie* página, el identificador de la película seleccionada se almacena en un campo oculto. (Se pasa en la página en primer lugar como un valor de cadena de consulta.) Hay un `Html.ValidationSummary` llamada que se mostrará los errores de validación. En este caso, el error puede ser que se ha pasado ningún identificador de película a la página o el identificador de película no es válido. Esta situación puede producirse si alguien ejecutó esta página sin activar primero una película en la *películas* página.

El título del botón es **eliminar película**, y su nombre de atributo se establece en `buttonDelete`. El `name` atributo se usará en el código para identificar el botón que envía el formulario.

Tendrá que escribir código para 1) leer los detalles de la película cuando se muestra la página por primera vez y (2) realmente elimina la película cuando el usuario hace clic en el botón.

## <a name="adding-code-to-read-a-single-movie"></a>Agregar código para leer una sola película

En la parte superior de la *DeleteMovie.cshtml* página, agregue el siguiente bloque de código:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Esta marca es el mismo que el código correspondiente en el *EditMovie* página. Obtiene el identificador de película fuera de la cadena de consulta y utiliza el identificador para leer un registro de la base de datos. El código incluye la prueba de validación (`IsInt()` y `row != null`) para asegurarse de que el identificador de película que se pasa a la página es válido.

Recuerde que este código sólo debe ejecutar la primera vez que se ejecuta la página. No desea volver a leer el registro de la película de la base de datos cuando el usuario hace clic en el **eliminar película** botón. Por lo tanto, el código para leer la película esté dentro de una prueba que dice `if(!IsPost)` &mdash; es decir, *si la solicitud no es una operación post (envío del formulario)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Agregar código para eliminar la película seleccionada

Para eliminar la película cuando el usuario hace clic en el botón, agregue el código siguiente justo dentro de la llave de cierre de la `@` bloque:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Este código es similar al código para actualizar un registro existente, pero es más sencillo. Básicamente, el código ejecuta una instancia de SQL `Delete` instrucción.

 Como en el *EditMovie* página, el código está en un `if(IsPost)` bloque. En esta ocasión, el `if()` condición es un poco más complicada: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Hay dos condiciones aquí. La primera es que se va a enviar la página, tal y como se ha visto antes &mdash; `if(IsPost)`.

La segunda condición es `!Request["buttonDelete"].IsEmpty()`, lo que significa que la solicitud tiene un objeto denominado `buttonDelete`. Es verdad, es una forma indirecta de probar qué botón enviado el formulario. Si un formulario contiene varios botones de envío, solo el nombre del botón que se ha hecho clic aparece en la solicitud. Por lo tanto, lógicamente, si el nombre de un botón determinado aparece en la solicitud &mdash; o como se indica en el código, si dicho botón no está vacío &mdash; que es el botón que envía el formulario.

El `&&` operador significa "and" (AND lógico). Por lo tanto, toda la matriz `if` condición es...

*Esta solicitud es una entrada de blog (no una solicitud de la primera vez)*  
  
 AND  
  
*El* `buttonDelete` *botón fue el botón que envía el formulario.*

Este formulario (de hecho, esta página) contiene un solo botón, por lo que la prueba adicional para `buttonDelete` técnicamente no es necesario. No obstante, está a punto de realizar una operación que se quitará de forma permanente los datos. Por lo que desea que estén tan seguro como sea posible que se va a realizar la operación solo cuando el usuario lo ha solicitado explícitamente. Por ejemplo, suponga que expande esta página más adelante y agregarle otros botones. Aún así, el código que elimina la película se ejecutará solo si el `buttonDelete` se hace clic en el botón.

Como en el *EditMovie* página, obtener el identificador del campo oculto y, a continuación, ejecute el comando SQL. La sintaxis de la `Delete` instrucción es:

`DELETE FROM table WHERE ID = value`

Es esencial para incluir la `WHERE` cláusula y el identificador. Si deja fuera de la cláusula WHERE, *se eliminarán todos los registros en la tabla*. Como ha visto, pase el valor de identificador para el comando SQL mediante el uso de un marcador de posición.

## <a name="testing-the-movie-delete-process"></a>Probar el proceso de eliminación de película

Ahora puede probar. Ejecute el *películas* página y haga clic en **eliminar** situada junto a una película. Cuando el *DeleteMovie* aparece en la página, haga clic en **eliminar película**.

![Eliminar página de la película con el botón Eliminar película resaltado](deleting-data/_static/image4.png)

Al hacer clic en el botón, el código elimina las películas y devuelve a la lista de películas. Se puede buscar la película se eliminó y confirme que se ha eliminado.

## <a name="coming-up-next"></a>Próxima

El siguiente tutorial muestra cómo proporcionar a todas las páginas en su sitio un aspecto y un diseño común.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Muestra la lista completa de página de la película (actualizada con vínculos eliminar)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Muestra la lista completa de página DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis de Razor](../introducing-razor-syntax-c.md)
- [Instrucción DELETE de SQL](http://www.w3schools.com/sql/sql_delete.asp) en el sitio W3Schools

> [!div class="step-by-step"]
> [Anterior](updating-data.md)
> [Siguiente](layouts.md)
