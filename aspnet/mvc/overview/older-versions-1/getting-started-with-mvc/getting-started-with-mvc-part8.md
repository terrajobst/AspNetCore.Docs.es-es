---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Agregar una columna al modelo | Documentos de Microsoft
author: shanselman
description: "Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Crear una aplicación web simple que lee y escribe desde una base de datos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 17ee105f596319423ac83cf718683ed293f952f3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-column-to-the-model"></a>Agregar una columna al modelo
====================
por [Scott Hanselman](https://github.com/shanselman)

> Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Se creará una aplicación web simple que lee y escribe desde una base de datos. Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección, vamos a recorrer cómo podemos realizar cambios en el esquema de la base de datos y controlar los cambios en nuestra aplicación.

Vamos a agregar a una columna "Clasificación" a la tabla de la película. Volver al IDE y haga clic en el Explorador de base de datos. Haga clic en la tabla de la película y seleccione Abrir definición de tabla.

Agregar una columna "Clasificación" tal como se muestra a continuación. Puesto que no tenemos ninguna clasificación ahora, la columna puede permitir valores NULL. Haga clic en Guardar.

[![Edición de películas de tabla](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

A continuación, vuelva al explorador de soluciones y abra el archivo Movies.edmx (que se encuentra en la carpeta \Models). Haga clic con el botón secundario en la superficie de diseño (el área blanca) y seleccione el modelo de actualización de base de datos.

[![Películas - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Se iniciará el Asistente para actualización de"". Haga clic en la pestaña de actualización dentro de él y haga clic en Finalizar. Nuestra clase de modelo de película, a continuación, se actualizará con la nueva columna.

![Asistente para actualización de (2)](getting-started-with-mvc-part8/_static/image5.png)

Después de hacer clic en Finalizar, puede ver que la nueva columna de clasificación se ha agregado a la entidad de la película en nuestro modelo.

[![Entidad de película](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Hemos agregado una columna en el modelo de base de datos, pero las vistas no saberlo.

## <a name="update-views-with-model-changes"></a>Actualizar vistas con los cambios de modelo

Hay varias maneras de podríamos actualizar nuestras plantillas de vista para reflejar la nueva columna de clasificación. Puesto que estas vistas se creó mediante generarlos mediante el cuadro de diálogo Agregar vista, podríamos eliminarlos y volver a crearlos de nuevo. Sin embargo, normalmente personas ya se habrá realizado modificaciones en las plantillas de vista de la generación con scaffolding inicial y desearán agregar o eliminar campos manualmente, como hicimos en el campo ID para crear.

Abra la plantilla de \Views\Movies\Index.aspx y agregue un &lt;th&gt;clasificación&lt;/th&gt; en el encabezado de la tabla de la película. Había agregado extraiga después de género. A continuación, en la misma posición de columna pero más abajo, agregue una línea para nuestra nueva clasificación de salida.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Nuestra plantilla Index.aspx final tendrá un aspecto similar al siguiente:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

A continuación, vamos a abrir la plantilla \Views\Movies\Create.aspx y agregar una etiqueta y un cuadro de texto para la nueva propiedad de clasificación:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

La plantilla Create.aspx final se este aspecto y vamos a cambiar el título y la base de datos secundaria de nuestro navegador &lt;h2&gt; título en algo similar a "Crear una película" mientras se encuentra aquí.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Ejecutar la aplicación y ahora ya tiene un campo nuevo en la base de datos que se han agregado a la página Crear. Agregar una película nueva - en este momento con una clasificación - y haga clic en crear.

[![Crear una película - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Después de hacer clic en crear, se envían a la página de índice donde se nuevos película aparece con la nueva columna de clasificación en la base de datos

[![Lista de películas - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Este tutorial básico de tenemos que pueda empezar a hacer que los controladores, asociarlos con las vistas y pasar sobre los datos codificados de forma rígida. A continuación, se crea y diseñado una base de datos y poner algunos datos en. Se recuperan los datos de la base de datos y se muestran en una tabla HTML. A continuación, agregamos un formulario de creación que permiten al usuario agregar datos a la base de datos por sí mismos desde dentro de la aplicación Web. Se agrega validación, a continuación, realice la validación usa JavaScript en el cliente. Por último, se cambia la base de datos para incluir una nueva columna de datos, a continuación, actualizan nuestras dos páginas para crear y mostrar los nuevos datos.

Ahora le animo a pasar a nuestro tutorial de nivel intermedio "[tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", así como los vídeos y recursos en muchas [https://asp.net/mvc](https://asp.net/mvc) para más información sobre ASP.NET MVC.

Disfrútelo.

- Scott Hanselman - [http://hanselman.com](http://hanselman.com) y [ @shanselman ](http://twitter.com/shanselman) en Twitter.

>[!div class="step-by-step"]
[Anterior](getting-started-with-mvc-part7.md)
