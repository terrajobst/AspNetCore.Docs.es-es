---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: "Agregar validación para el modelo | Documentos de Microsoft"
author: shanselman
description: "Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Se creará una aplicación web simple que lee y escribe desde una base de datos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 25c939bc8121589f91914e553d56e8f0975115b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model"></a>Agregar validación para el modelo
====================
por [Scott Hanselman](https://github.com/shanselman)

> Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Se creará una aplicación web simple que lee y escribe desde una base de datos. Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección, vamos a implementar la compatibilidad necesaria para habilitar la validación de entrada en nuestra aplicación. Se garantizará que nuestro contenido de la base de datos siempre es correcta y proporcionan mensajes de error útiles a los usuarios finales cuando intentan y escriba los datos de la película que no es válidos. Comenzaremos agregando un poco lógica de validación a la clase de la película.

Haga clic con el botón secundario en la carpeta del modelo y seleccione Agregar clase. Nombre de la clase de la película.

Cuando se creó el modelo de entidad de película anteriormente, el IDE ha creado una clase de la película. De hecho, puede ser parte de la clase de la película en un archivo y parte de otro. Esto se denomina una clase parcial. Vamos a ampliar la clase de película desde otro archivo.

Vamos a crear una clase parcial de película que apunta a una "clase relacionada" con algunos atributos que le proporcionará sugerencias de validación en el sistema. Comenzaremos marcar el título y el precio según sea necesario y también insistir en que el precio sea dentro de un intervalo determinado. Haga clic en la carpeta de modelos y seleccione Agregar clase. Nombre de la clase de película y haga clic en el botón Aceptar. Este es nuestro parcial película clase aspecto.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Vuelva a ejecutar la aplicación e intente escribir una película con un precio superiores a 100. Una vez que ha enviado el formulario, obtendrá un error. El error se captura en el servidor y se produce después de que se publique el formulario. Tenga en cuenta cómo aplicaciones auxiliares HTML integradas de ASP.NET MVC eran lo suficientemente inteligentes como para mostrar el mensaje de error y mantener los valores para que podamos dentro de los elementos de cuadro de texto:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Esto funciona bien, pero sería estupendo si podemos decirle al usuario en el lado cliente, inmediatamente, antes de que el servidor obtiene implicado.

Vamos a habilitar alguna validación del lado cliente con JavaScript.

## <a name="adding-client-side-validation"></a>Agregar la validación del lado cliente

Puesto que nuestra clase película ya tiene algunos atributos de validación, se necesitará agregar algunos archivos de JavaScript a nuestra plantilla Create.aspx vista y agregar una línea de código para habilitar la validación del lado cliente que se realicen.

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

Ejecutar la aplicación, visite /Movies/Create nuevo y haga clic en crear sin escribir ningún dato. Los mensajes de error aparecen inmediatamente sin la página flash que asociamos con el envío de datos de todas la manera de nuevo al servidor. Esto es porque ASP.NET MVC ahora se está validando la entrada tanto en el cliente (mediante JavaScript) y en el servidor.

[![Crear - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

¡Este es otro aspecto! Agreguemos ahora una columna adicional a la base de datos.

>[!div class="step-by-step"]
[Anterior](getting-started-with-mvc-part6.md)
[Siguiente](getting-started-with-mvc-part8.md)
