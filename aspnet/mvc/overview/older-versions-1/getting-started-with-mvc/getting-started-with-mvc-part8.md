---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Agregar una columna al modelo | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 5dbda1280c073d5d4f2d71918ca31bc44475f64d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817206"
---
<a name="adding-a-column-to-the-model"></a>Agregar una columna al modelo
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe desde una base de datos. Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección vamos a recorrer cómo podemos realizar cambios en el esquema de nuestra base de datos y controlar los cambios dentro de nuestra aplicación.

Vamos a agregar a una columna "Clasificación" a la tabla de la película. Vuelva al IDE y haga clic en el Explorador de base de datos. Haga clic en la tabla de la película y seleccione Abrir definición de tabla.

Agregar una columna de "Rating" tal como se muestra a continuación. Puesto que no hay ninguna clasificación ahora, la columna puede permitir valores NULL. Haga clic en Guardar.

[![Edición de la tabla de películas](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

A continuación, vuelva al explorador de soluciones y abra el archivo Movies.edmx (que se encuentra en la carpeta \Models). Haga clic con el botón derecho en la superficie de diseño (el área blanca) y seleccione el modelo de actualización de base de datos.

[![Películas - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Esto iniciará al Asistente de actualización de"". Haga clic en la pestaña de actualización dentro de él y haga clic en Finalizar. Nuestra clase de modelo de película, a continuación, se actualizará con la nueva columna.

![Asistente para actualizar (2)](getting-started-with-mvc-part8/_static/image5.png)

Después de hacer clic en Finalizar, puede ver que la nueva columna de clasificación se ha agregado a la entidad de la película en nuestro modelo.

[![Entidad de película](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Hemos agregado una columna en el modelo de base de datos, pero no conocen las vistas.

## <a name="update-views-with-model-changes"></a>Actualizar las vistas con los cambios del modelo

Hay varias maneras de podríamos actualizamos nuestras plantillas de vista para reflejar la nueva columna de clasificación. Puesto que hemos creado estas vistas mediante la generación de mediante el cuadro de diálogo Agregar vista, podríamos eliminarlas y volver a crearlas de nuevo. Sin embargo, normalmente las personas ya se habrá realizado modificaciones en sus plantillas de vista de la generación inicial con scaffolding y conveniente agregar o eliminar campos manualmente, tal como hicimos con el campo Id. para crear.

Abra la plantilla \Views\Movies\Index.aspx y agregue un &lt;th&gt;clasificación&lt;/th&gt; en el encabezado de la tabla Movie. Agregué los míos después de género. A continuación, en la misma posición de columna pero más abajo, agregue una línea para nuestra nueva clasificación de salida.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Nuestra plantilla Index.aspx final tendrá un aspecto similar al siguiente:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

A continuación, vamos a abrir la plantilla \Views\Movies\Create.aspx y agregar una etiqueta y un cuadro de texto para la nueva propiedad de clasificación:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Este aspecto el nuestra plantilla Create.aspx final y vamos a cambiar el título y la base de datos secundaria de nuestro explorador &lt;h2&gt; título a algo parecido a "Crear una película" mientras estamos aquí!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Ejecute la aplicación y ahora tenemos un nuevo campo en la base de datos que se han agregado a la página Create. Agregar una nueva película - esta vez con una clasificación - y haga clic en crear.

[![Crear una película - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Después de hacer clic en crear, se le envía a la página de índice donde se nueva película aparece con la nueva columna de clasificación en la base de datos

[![Lista de películas - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Este tutorial básico de lo que a realizar los controladores de su asociación con las vistas y pasan datos codificados de forma rígida. A continuación, creamos y diseñado una base de datos y colocar algunos datos en. Se recuperan los datos de la base de datos y se muestra en una tabla HTML. A continuación, agregamos un formulario de creación que permiten al usuario que se agreguen datos a la base de datos desde dentro de la aplicación Web. Se agregó la validación y luego realiza la validación de usar JavaScript en el lado cliente. Por último, se puede cambiar la base de datos para incluir una nueva columna de datos y luego actualizan nuestras dos páginas para crear y mostrar estos nuevos datos.

Ahora le animo a pasar a nuestro tutorial de nivel intermedio "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", así como los vídeos y recursos en muchas [ https://asp.net/mvc ](https://asp.net/mvc) para obtener más información sobre ASP.NET MVC.

Disfrútelo.

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) y [ @shanselman ](http://twitter.com/shanselman) en Twitter.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part7.md)
