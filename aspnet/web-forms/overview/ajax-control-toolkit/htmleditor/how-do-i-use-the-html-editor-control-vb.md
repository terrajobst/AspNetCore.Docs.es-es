---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: "¿Cómo se puede utilizar el Editor HTML Control? (VB) | Documentos de Microsoft"
author: microsoft
description: "HTMLEditor es un Control de AJAX de ASP.NET que permite crear y editar con facilidad contenido HTML a través de los botones en una barra de herramientas."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a34b3dd53f031856906eca923b6ad6f43a0aaecc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>¿Cómo se puede utilizar el Editor HTML Control? (VB)
====================
por [Microsoft](https://github.com/microsoft)

> HTMLEditor es un Control de AJAX de ASP.NET que permite crear y editar con facilidad contenido HTML a través de los botones en una barra de herramientas.


El objetivo de este tutorial es proporcionar información general sobre el control de Editor HTML incluido con el Kit de herramientas de Control de AJAX. El Editor HTML incluye opciones para cambiar el tamaño de fuente, seleccione una fuente, cambiar el color de fondo, modificar el color de primer plano, la adición de vínculos, agregar imágenes, cambiar la alineación del texto y llevar a cabo cortar, copiar y pegar operaciones (consulte la figura 1).


[![El Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Figura 01**: el Editor de HTML ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


El editor HTML le permite escribir contenido con un modo de diseño o puede escribir HTML directamente. También se proporcionan con la opción para obtener una vista previa de su contenido HTML (consulte la figura 2).


[![Diseño, HTML y vista previa de botones](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Figura 02**: botones de diseño, HTML y vista previa ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


En este tutorial, aprenderá cómo mostrar el Editor de HTML, cómo personalizar los botones de barra de herramientas que aparecen en el Editor de HTML y cómo evitar los ataques de Scripting entre sitios.

## <a name="displaying-the-html-editor"></a>Mostrar el Editor de HTML

Para poder usar el Editor de HTML en una página ASP.NET, primero debe agregar un control ScriptManager a la página. El control ScriptManager se encuentra debajo de la ficha Extensiones AJAX en el cuadro de herramientas de Visual Studio o Visual Web Developer Express.

Debe colocar el control ScriptManager en la parte superior de la página antes que los otros controles en la página. Por ejemplo, puede colocarlo inmediatamente por debajo de la apertura servidor &lt;formulario&gt; etiqueta.

El control del Editor de HTML se encuentra en el cuadro de herramientas con el resto de los controles del Kit de herramientas de Control de AJAX. Se denomina el control del Editor (consulte la figura 3).


[![El control del Editor de HTML](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Figura 03**: el Editor de HTML control ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


Después de arrastrar el Editor de HTML en una página, puede establecer sus propiedades en la hoja de propiedades. Por ejemplo, normalmente desea establecer las propiedades de ancho y alto. Lista 1 contiene el origen de una página ASP.NET que contiene un editor de HTML.

**Lista 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

La página del listado 1 contiene un control de Editor de HTML, un control de botón y un control del Literal. Al hacer clic en el botón, el contenido del Editor HTML aparece en el control del Literal (consulte la figura 4).


[![Enviar un formulario con un Editor de HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Figura 04**: enviar un formulario con un Editor de HTML ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


La propiedad Content del Editor HTML se usa para recuperar el contenido HTML especificado en el Editor de HTML. Tenga en cuenta que este contenido HTML puede contener JavaScript. En la siguiente sección, se describe cómo se pueden impedir los ataques de inyección de código de JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalizar la barra de herramientas del Editor HTML

Puede personalizar exactamente los botones que aparecen en el editor. Por ejemplo, puede quitar la ficha HTML para evitar que los usuarios cambien el Editor HTML en el modo de HTML. O bien, puede quitar la lista desplegable de tamaño de fuente para impedir que los usuarios creen excesivamente grande de texto en un foro de mensaje post (consulte la figura 5).


[![Editor HTML personalizado](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Figura 05**: A personalizar el Editor HTML ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


Personalizar los botones de barra de herramientas al derivar un nuevo Editor de HTML de la clase base del Editor. Por ejemplo, el editor personalizado en el listado 2 solo contiene botones de barra de herramientas para negrita y cursiva. Se han quitado todos los demás botones de barra de herramientas. Además, la pestaña HTML se ha quitado de la parte inferior del editor (pero las fichas Diseño y vista previa siguen estando disponibles).

**La lista 2 - aplicación\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

Debe agregar la clase en el listado 2 a la aplicación\_carpeta de código para que la clase se va a compilar automáticamente. Si la aplicación\_carpeta de código no existe en el sitio Web, a continuación, simplemente puede agregar la carpeta.

Después de crear un editor personalizado, puede agregarlo a una página ASP.NET de la misma manera a medida que agregue el Editor de HTML normal (consulte el listado 3).

**El listado 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Evitar ataques de Scripting entre sitios (XSS)

Siempre que aceptan la entrada de un usuario y volver a mostrar esa entrada en el sitio Web, podría abrir el sitio Web a los ataques de Scripting entre sitios (XSS). En teoría, un usuario malintencionado podría enviar código JavaScript que se ejecuta cuando se vuelve a mostrar la entrada. El código JavaScript podría utilizarse para robar las contraseñas de usuario u otra información confidencial.

Normalmente, puede anular ataques XSS codificación cualquier entrada recuperar de un usuario antes de mostrar en una página web HTML. Sin embargo, la salida del Editor HTML de codificación HTML no debería codificar solo &lt;script&gt; etiquetas, también podrían codificar todas las etiquetas HTML. En otras palabras, se perderá todo el formato, como el tipo de fuente, tamaño de fuente y color de fondo.

Si va a recopilar información confidencial de los usuarios, como contraseñas, números de tarjeta de crédito y números de la seguridad social - no deben mostrar contenido sin codificar que recupere de un usuario con el Editor de HTML. Debe usar el Editor HTML solo en situaciones en las que se vuelva a mostrar no el contenido HTML o el contenido HTML que se envía a su sitio Web por un usuario de confianza.

Por ejemplo, imagine que está creando una aplicación de blog. En esta situación, tiene sentido usar el Editor HTML al crear las entradas de blog. Es la única persona que envía una entrada de blog y, supuestamente, puede confiar en sí mismo no para enviar código JavaScript malintencionado. Sin embargo, que tiene sentido usar el Editor HTML al permitir que los usuarios anónimos enviar comentarios. Debe tener especial cuidado en situaciones en que los usuarios envían información confidencial como contraseñas. Potencialmente, un usuario malintencionado podría exponer un comentario que contiene el código de JavaScript derecho de robo de una contraseña.

## <a name="summary"></a>Resumen

En este tutorial, se proporciona una breve descripción general del control de Editor HTML incluido en el Kit de herramientas de Control de AJAX. Aprendió a utilizar el Editor HTML para aceptar contenido enriquecido de un usuario y enviar el contenido en el servidor. También explicamos cómo personalizar los botones de barra de herramientas que se muestran con el Editor HTML. Por último, aprendió a evitar los ataques de Scripting entre sitios al usar el Editor HTML para aceptar datos potencialmente dañinos.

>[!div class="step-by-step"]
[Anterior](how-do-i-use-the-html-editor-control-cs.md)
