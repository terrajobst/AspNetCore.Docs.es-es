---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Agregar validación al modelo | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: f8172b63fcaa7599f5dd733271d9ea7570e3bcb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802791"
---
<a name="adding-validation-to-the-model"></a>Agregar validación al modelo
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe desde una base de datos. Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección vamos a implementar la compatibilidad necesaria para habilitar la validación de entrada dentro de nuestra aplicación. Le garantizamos que el contenido de la base de datos siempre es correcto y proporcionar mensajes de error útiles a los usuarios finales cuando pruebe y escriba los datos de la película que no es válidos. Empezaremos por agregar lógica de validación de un poco a la clase Movie.

Haga clic con el botón derecho en la carpeta de modelo y seleccione Agregar clase. Nombre de la clase Movie.

Cuando se creó el modelo de entidades de película, el IDE crea una clase llamada Movie. De hecho, puede ser parte de la clase de la película en un archivo y parte en otro. Esto se denomina una clase parcial. Vamos a ampliar la clase Movie de otro archivo.

Vamos a crear una clase parcial de película que apunta a un amigo "class" con algunos atributos que proporcionan las sugerencias de validación en el sistema. Se deberá marcar el título y el precio según sea necesario y también insistir en que el precio ser dentro de un intervalo determinado. A la derecha, haga clic en la carpeta Models y seleccione Agregar clase. Nombre de la clase Movie y haga clic en el botón Aceptar. Este es nuestro parcial película clase aspecto.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Vuelva a ejecutar la aplicación e intenta insertar una película con un precio superior a 100. Obtendrá un error después de haber enviado el formulario. El error se captura en el servidor y se produce después de que el formulario se publica. Tenga en cuenta cómo aplicaciones auxiliares de HTML integradas de ASP.NET MVC eran suficientemente inteligentes como para mostrar el mensaje de error y mantenga los valores para que podamos dentro de los elementos de cuadro de texto:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Esto funciona bien, pero sería bueno si podemos decirle al usuario en el lado cliente, inmediatamente, antes de que el servidor se implique.

Vamos a habilitar alguna validación del lado cliente con JavaScript.

## <a name="adding-client-side-validation"></a>Agregar la validación del lado cliente

Dado que nuestra clase Movie ya tiene algunos atributos de validación, sólo necesitaremos agregar algunos archivos de JavaScript a nuestra plantilla de vista Create.aspx y agregar una línea de código para habilitar la validación del lado cliente que tenga lugar.

Desde VWD vaya la carpeta vistas/película y abra Create.aspx.

Abra la carpeta de Scripts en el Explorador de soluciones y arrastre las siguientes tres secuencias de comandos dentro de la &lt;head&gt; etiqueta.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Desea que estos archivos de script que aparecen en este orden.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Además, agregue esta línea por encima de la Html.BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Este es el código que se muestra en el IDE.

[![Películas - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Ejecute la aplicación y vuelve a visitar /Movies/Create y haga clic en crear sin especificar ningún dato. Los mensajes de error aparecen inmediatamente sin la página que se asocia con el envío de datos de flash remontándose al servidor. Esto es porque ASP.NET MVC ahora se está validando la entrada tanto en el cliente (mediante JavaScript) y en el servidor.

[![Crear - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

¡Esto es genial! Ahora agreguemos una columna adicional a la base de datos.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part6.md)
> [Siguiente](getting-started-with-mvc-part8.md)
