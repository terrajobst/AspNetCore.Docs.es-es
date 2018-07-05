---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: ¿Cómo se puede usar el Editor HTML Control? (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor es un Control de AJAX de ASP.NET que le permite crear fácilmente y editar el contenido HTML a través de los botones de una barra de herramientas.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: bff352161693c5e3871c20b8b469fb09f473686e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801533"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>¿Cómo se puede usar el Editor HTML Control? (VB)
====================
por [Microsoft](https://github.com/microsoft)

> HTMLEditor es un Control de AJAX de ASP.NET que le permite crear fácilmente y editar el contenido HTML a través de los botones de una barra de herramientas.


El objetivo de este tutorial es proporcionarle una visión general del control de Editor HTML incluido con AJAX Control Toolkit. El Editor HTML incluye opciones para cambiar el tamaño de fuente, seleccionar una fuente, cambiar el color de fondo, modificar el color de primer plano, la adición de vínculos, agregar imágenes, cambiar la alineación del texto y realización de cortar, copiar y pegar (consulte la figura 1) de las operaciones.


[![El Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Figura 01**: el Editor de HTML ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


El editor HTML le permite especificar el contenido mediante un modo de diseño o puede escribir HTML directamente. También se proporcionan con la opción para obtener una vista previa de su contenido HTML (consulte la figura 2).


[![Vista previa, HTML y diseño de botones](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Figura 02**: botones de diseño, HTML y vista previa ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


En este tutorial, obtendrá información sobre cómo mostrar el Editor HTML, cómo personalizar los botones de barra de herramientas que aparecen en el Editor de HTML y cómo evitar la realización de ataques entre sitios.

## <a name="displaying-the-html-editor"></a>Mostrar el Editor de HTML

Para poder usar el Editor de HTML en una página ASP.NET, primero debe agregar un control ScriptManager en la página. El control ScriptManager se encuentra debajo de la pestaña Extensiones de AJAX en el cuadro de herramientas de Visual Studio o Visual Web Developer Express.

Debe colocar el control ScriptManager en la parte superior de la página antes que los otros controles en la página. Por ejemplo, puede colocarlo inmediatamente debajo de la apertura del servidor &lt;formulario&gt; etiqueta.

El control de Editor HTML se encuentra en el cuadro de herramientas con el resto de los controles de AJAX Control Toolkit. Se llama el control del Editor (consulte la figura 3).


[![El control de Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Figura 03**: control The Editor HTML ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


Después de arrastrar el Editor de HTML a una página, puede establecer sus propiedades en la hoja de propiedades. Por ejemplo, normalmente desea establecer las propiedades Width y Height. Listado 1 contiene el origen de una página ASP.NET que contiene un editor de HTML.

**Listado 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

La página del listado 1 contiene un control de Editor HTML, un control de botón y un control Literal. Al hacer clic en el botón, el contenido del Editor HTML aparece en el control Literal (consulte la figura 4).


[![Enviar un formulario con un Editor de HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Figura 04**: enviar un formulario con un Editor de HTML ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


La propiedad Editor HTML Content se usa para recuperar el contenido HTML especificado en el Editor de HTML. Tenga en cuenta que este contenido HTML puede contener JavaScript. En la siguiente sección, trataremos cómo puede impedir los ataques por inyección de código de JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalizar la barra de herramientas del Editor HTML

Puede personalizar exactamente los botones que aparecen en el editor. Por ejemplo, es posible que desee quitar la ficha HTML para evitar que los usuarios cambien el Editor HTML en el modo HTML. O bien, es posible que desea quitar de la lista desplegable de tamaño de fuente para evitar que los usuarios crear excesivamente grande de texto en un foro de publicación de mensajes (consulte la figura 5).


[![Editor HTML personalizado](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Figura 05**: A personalizar el Editor HTML ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


Personalizar los botones de barra de herramientas al derivar un nuevo Editor de HTML de la clase base del Editor. Por ejemplo, el editor personalizado en el listado 2 contiene solo los botones de barra de herramientas de negrita y cursiva. Se han quitado todos los demás botones de barra de herramientas. Además, la ficha HTML se ha quitado de la parte inferior del editor (pero las fichas Diseño y vista previa siguen estando disponibles).

**Listado 2 - App\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

Debe agregar la clase a la aplicación en el listado 2\_carpeta de código para que la clase se compilará automáticamente. Si la aplicación\_carpeta de código no existe en el sitio Web, a continuación, simplemente puede agregar la carpeta.

Después de crear un editor personalizado, puede agregarlo a una página ASP.NET en la misma manera a medida que agrega el Editor de HTML normal (consulte el listado 3).

**Listado 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Evitar ataques de Scripting entre sitios (XSS)

Cada vez que se aceptan la entrada de un usuario y volver a mostrar esa entrada en el sitio Web, podría abrir su sitio Web a los ataques de Scripting entre sitios (XSS). En teoría, un usuario malintencionado podría enviar código de JavaScript que se ejecuta cuando se vuelve a mostrar la entrada. El código JavaScript podría usarse para robar contraseñas de usuario u otra información confidencial.

Normalmente, puede anular los ataques XSS de codificación cualquier entrada que recuperar de un usuario antes de mostrarlos en una página web HTML. Sin embargo, la salida del Editor HTML de codificación HTML no sólo codificaría &lt;script&gt; etiquetas, también codificaría todas las etiquetas HTML. En otras palabras, se pierden todos los formatos, como el tipo de fuente, tamaño de fuente y color de fondo.

Si está recopilando información confidencial de sus usuarios, como contraseñas, números de tarjeta de crédito y números del seguro social - no deben mostrar contenido sin codificar que recupere de un usuario con el Editor de HTML. Debe usar el Editor HTML solo en situaciones en que no son volver a mostrar el contenido HTML o contenido HTML que se envía a su sitio Web por un usuario de confianza.

Por ejemplo, imagine que va a crear una aplicación de blog. En esta situación, tiene sentido utilizar el Editor de HTML al crear entradas de blog. Son la única persona que envía una entrada de blog y, presumiblemente, puede confiar usted mismo, no para enviar código JavaScript malintencionado. Sin embargo, no sentido usar el Editor HTML al permitir que los usuarios anónimos publicar comentarios. Debe ser especialmente cuidadoso en situaciones en que los usuarios enviar información confidencial como contraseñas. Potencialmente, un usuario malintencionado podría publicar un comentario que contiene el código de JavaScript adecuado de robo de una contraseña.

## <a name="summary"></a>Resumen

En este tutorial, se proporciona una breve descripción general del control de Editor HTML incluido en el AJAX Control Toolkit. Ha aprendido a usar el Editor de HTML para aceptar contenido enriquecido de un usuario y enviar el contenido en el servidor. También analizamos cómo puede personalizar los botones de barra de herramientas que se muestran con el Editor HTML. Por último, ha aprendido a evitar ataques de scripts entre sitios al utilizar el Editor de HTML para aceptar la entrada potencialmente malintencionado.

> [!div class="step-by-step"]
> [Anterior](how-do-i-use-the-html-editor-control-cs.md)
