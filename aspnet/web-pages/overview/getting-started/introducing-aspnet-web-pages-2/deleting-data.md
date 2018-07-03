---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: 'Introducción a ASP.NET Web Pages: eliminación de la base de datos | Microsoft Docs'
author: tfitzmac
description: Este tutorial muestra cómo eliminar una entrada de la base de datos individual. Supone que ha completado la serie a través de actualizar los datos de base de datos de ASP.NET Web PA...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 45cd3ed7fdcede05823ef28d7cc6c8da3922dad7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362095"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Introducción a ASP.NET Web Pages: eliminación de la base de datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo eliminar una entrada de la base de datos individual. Supone que ha completado la serie a través de [actualizar la base de datos en ASP.NET Web Pages](updating-data.md).
> 
> Lo que aprenderá:
> 
> - Cómo seleccionar un registro individual en una lista de registros.
> - Cómo eliminar un único registro de una base de datos.
> - Cómo comprobar que se hizo clic en un botón específico en un formulario.
>   
> 
> Características y tecnologías tratadas:
> 
> - El `WebGrid` auxiliar.
> - El código SQL `Delete` comando.
> - El `Database.Execute` método para ejecutar una instancia de SQL `Delete` comando.


## <a name="what-youll-build"></a>¿Qué va a crear

En el tutorial anterior, aprendió a actualizar un registro de base de datos existente. Este tutorial es similar, salvo que en lugar de actualizar el registro, va a eliminar. Los procesos son muy similares, salvo que la eliminación es más sencillo, para que este tutorial sea breve.

En el *películas* página, actualizará la `WebGrid` auxiliar para que muestre que TI un **eliminar** vínculo situado junto a cada película para acompañar la **editar** vínculo agregado anteriormente.

![Página de películas que muestra un vínculo de eliminación de cada película](deleting-data/_static/image1.png)

Al igual que con la edición, al hacer clic en el **eliminar** vínculo le lleva a una página distinta, donde la información de películas ya está en un formulario:

![Eliminar página de la película con una película muestra](deleting-data/_static/image2.png)

A continuación, puede hacer clic en el botón para eliminar permanentemente el registro.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Agregar un vínculo de eliminación a la lista de películas

Empezará agregando un **eliminar** vincular a la `WebGrid` auxiliar. Este vínculo es similar a la **editar** vínculo que agregó en un tutorial anterior.

Abra el *Movies.cshtml* archivo.

Cambiar el `WebGrid` marcado en el cuerpo de la página mediante la adición de una columna. Aquí está el marcado modificado:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Esta es la nueva columna:

[!code-html[Main](deleting-data/samples/sample2.html)]

La forma en la cuadrícula está configurada, el **editar** columna es la parte izquierda de la cuadrícula y el **eliminar** columna es más a la derecha. (Hay una coma después de la `Year` columna ahora, en caso de no tenga en cuenta que.) No hay nada especial acerca de dónde ir estas columnas de vínculo y fácilmente podría poner junto a la otra. En este caso, son independientes para hacerlos más difíciles se mezclen.

![Marcado de página de películas con vínculos de edición y los detalles para mostrar que no son contiguos](deleting-data/_static/image3.png)

La nueva columna muestra un vínculo (`<a>` elemento) cuyo texto dice "Eliminar". El destino del vínculo (su `href` atributo) es el código que en última instancia se resuelve como algo parecido a esta dirección URL, con el `id` valor diferente para cada película:

[!code-css[Main](deleting-data/samples/sample3.css)]

Este vínculo invocará una página denominada *DeleteMovie* y pásele el identificador de la película que ha seleccionado.

En este tutorial no entraré en detalles acerca de cómo se construye este vínculo, porque es casi idéntica a la **editar** vínculo desde el tutorial anterior ([actualizar la base de datos en ASP.NET Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Creación de la página Delete

Ahora puede crear la página que será el destino de la **eliminar** vínculo en la cuadrícula.

> [!NOTE] 
> 
> **Importante** la técnica de selecciona primero un registro para eliminar y, a continuación, con una página independiente y un botón para confirmar el proceso es muy importante para la seguridad. Como se ha leído en los tutoriales anteriores, lo *cualquier* tipo de cambio en su sitio Web debe *siempre* realizarse mediante un formulario &mdash; es decir, usando una operación HTTP POST. Si es posible cambiar el sitio simplemente haciendo clic en un vínculo (es decir, con una operación GET), las personas podrían realizar solicitudes sencillas en el sitio y eliminar los datos. Incluso un motor de búsqueda rastreador (crawler) que se está indizando el sitio podría eliminar accidentalmente los datos por los vínculos siguientes.
> 
> Cuando la aplicación permite a los usuarios cambiar el registro, deberá presentar el registro al usuario para la edición de todos modos. Pero podría verse tentado a omitir este paso para eliminar un registro. No omita este paso, sin embargo. (También es útil para los usuarios vean el registro y confirme que está eliminando el registro que está previsto).
> 
> En un conjunto de tutoriales posteriores, verá cómo agregar funcionalidad de inicio de sesión para que un usuario tendría que iniciar sesión antes de eliminar un registro.


Cree una página denominada *DeleteMovie.cshtml* y sustituir lo que aparece en el archivo con el siguiente marcado:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Este marcado es similar a la *EditMovie* páginas, salvo que en lugar de usar los cuadros de texto (`<input type="text">`), las marcas incluyen `<span>` elementos. No hay nada aquí para editar. Lo único que debe hacer es mostrar los detalles de la película para que los usuarios pueden asegurarse de que está eliminando la película de la derecha.

El marcado ya contiene un vínculo que permite al usuario volver a la página de listado de películas.

Como en el *EditMovie* página, el identificador de la película seleccionada se almacena en un campo oculto. (Se pasa en la página en primer lugar como un valor de cadena de consulta.) Hay un `Html.ValidationSummary` llamada que se mostrará los errores de validación. En este caso, el error puede ser que se ha pasado ningún identificador de película a la página o el Id. de película no es válido. Esta situación puede producirse si alguien se ha ejecutado esta página sin seleccionar primero una película en el *películas* página.

Es el título del botón **eliminar película**, y su nombre de atributo se establece en `buttonDelete`. El `name` atributo se usará en el código para identificar el botón que envía el formulario.

Tendrá que escribir código para 1) leer los detalles de la película cuando la página se muestra por primera vez y se eliminan (2) realmente la película cuando el usuario hace clic en el botón.

## <a name="adding-code-to-read-a-single-movie"></a>Agregar código para leer una película única

En la parte superior de la *DeleteMovie.cshtml* página, agregue el siguiente bloque de código:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Este marcado es el mismo que el código correspondiente en el *EditMovie* página. Obtiene el identificador de película fuera de la cadena de consulta y el identificador se utiliza para leer un registro de la base de datos. El código incluye la prueba de validación (`IsInt()` y `row != null`) para asegurarse de que el Id. de película que se pasan a la página es válido.

Recuerde que este código sólo se debe ejecutar la primera vez que se ejecuta la página. No desea volver a leer el registro de la película de la base de datos cuando el usuario hace clic en el **eliminar película** botón. Por lo tanto, el código para leer la película está dentro de una prueba que dice `if(!IsPost)` &mdash; es decir, *si la solicitud no es una operación post (envío de formulario)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Agregar código para eliminar la película seleccionada

Para eliminar la película cuando el usuario hace clic en el botón, agregue el código siguiente justo dentro de la llave de cierre de la `@` bloque:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Este código es similar al código para actualizar un registro existente, pero es más sencillo. Básicamente, el código ejecuta una instancia de SQL `Delete` instrucción.

 Como en el *EditMovie* página, el código está en un `if(IsPost)` bloque. Esta vez, el `if()` condición es un poco más complicado: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Hay dos condiciones aquí. La primera es que se está enviando la página, como ha visto antes &mdash; `if(IsPost)`.

La segunda condición es `!Request["buttonDelete"].IsEmpty()`, lo que significa que la solicitud tiene un objeto denominado `buttonDelete`. Es verdad, es una forma indirecta de pruebas de botón que envía el formulario. Si un formulario contiene varios botones de envío, sólo el nombre del botón que se hizo clic aparece en la solicitud. Por lo tanto, lógicamente, si el nombre de un botón determinado aparece en la solicitud &mdash; o como se indicó en el código, si ese botón no está vacío &mdash; que es el botón que envía el formulario.

El `&&` operador significa "y" (AND lógico). Por lo tanto, toda la `if` condición es...

*Esta solicitud es una entrada de blog (no una solicitud por primera vez)*  
  
 AND  
  
*El* `buttonDelete` *botón fue el botón que envía el formulario.*

Este formulario (de hecho, esta página) contiene sólo un botón, por lo que la prueba adicional para `buttonDelete` técnicamente no es necesario. Aun así, va a realizar una operación que se quitará permanentemente los datos. ¿Desea ser tan seguro como sea posible realizar la operación solo cuando el usuario lo ha solicitado explícitamente. Por ejemplo, suponga que esta página puede expandir más adelante y agregarán otros botones. Incluso entonces, el código que elimina la película se ejecutará solo si el `buttonDelete` botón se hizo clic.

Como en el *EditMovie* página, obtener el identificador del campo oculto y, a continuación, ejecute el comando SQL. La sintaxis para el `Delete` instrucción es:

`DELETE FROM table WHERE ID = value`

Es fundamental para incluir el `WHERE` cláusula y el identificador. Si deja fuera de la cláusula WHERE, *se eliminarán todos los registros de la tabla*. Como ha visto, pase el valor de identificador para el comando SQL mediante el uso de un marcador de posición.

## <a name="testing-the-movie-delete-process"></a>Probar el proceso de eliminación de película

Ahora puede probar. Ejecute el *películas* página y haga clic en **eliminar** junto a una película. Cuando el *DeleteMovie* aparece en la página, haga clic en **eliminar película**.

![Eliminar página de la película con el botón Eliminar película resaltado](deleting-data/_static/image4.png)

Al hacer clic en el botón, el código elimina las películas y lo devuelve a la lista de películas. Ahí puede buscar la película se eliminó y confirme que se ha eliminado.

## <a name="coming-up-next"></a>Próxima

El siguiente tutorial muestra cómo dar todas las páginas en el sitio una apariencia y el diseño común.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Lista completa de la página de película (actualizada con vínculos de eliminación)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Lista completa de página DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis de Razor](../introducing-razor-syntax-c.md)
- [Instrucción DELETE de SQL](http://www.w3schools.com/sql/sql_delete.asp) en el sitio W3Schools

> [!div class="step-by-step"]
> [Anterior](updating-data.md)
> [Siguiente](layouts.md)
