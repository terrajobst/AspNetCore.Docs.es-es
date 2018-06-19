---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: Varios ContentPlaceHolders y contenido predeterminado (C#) | Documentos de Microsoft
author: rick-anderson
description: Examina cómo agregar varios marcadores de posición de contenido a una página maestra, así como cómo especificar contenido predeterminado en los marcadores de posición de contenido.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: b60017c21b4cf45081893af08e68186009475fd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889061"
---
<a name="multiple-contentplaceholders-and-default-content-c"></a>Varios ContentPlaceHolders y contenido predeterminado (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Examina cómo agregar varios marcadores de posición de contenido a una página maestra, así como cómo especificar contenido predeterminado en los marcadores de posición de contenido.


## <a name="introduction"></a>Introducción

En el tutorial anterior se examina cómo habilitar las páginas maestras a los desarrolladores ASP.NET para crear un diseño coherente de todo el sitio. Páginas maestras definen áreas que son personalizables según una página a página y marcado que es común a todas sus páginas de contenido. En el tutorial anterior, creamos una simple página maestra (`Site.master`) y dos páginas de contenido (`Default.aspx` y `About.aspx`). Nuestra página maestra constaba de dos ContentPlaceHolders denominados `head` y `MainContent`, que se encuentra en la `<head>` elemento y el formulario Web Forms, respectivamente. Mientras que las páginas de contenido tenían dos controles de contenido, hemos especificado sólo marcado por el que corresponde a `MainContent`.

Como pone de manifiesto los dos controles ContentPlaceHolder en `Site.master`, una página maestra puede contener varios ContentPlaceHolders. Es más, la página maestra puede especificar marcado predeterminado para los controles ContentPlaceHolder. Una página de contenido, a continuación, puede, opcionalmente, especificar su propio marcado o utilizar el marcado de forma predeterminada. En este tutorial consultamos utilizando varios controles de contenido en la página maestra y vea cómo definir el marcado de forma predeterminada en los controles ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Paso 1: Agregar controles ContentPlaceHolder adicionales a la página maestra

Muchos diseños de sitio Web contienen varias áreas en la pantalla que se personalizan según una página a página. `Site.master`, la página maestra que se creó en el tutorial anterior, contiene un único ContentPlaceHolder dentro del formulario Web denominado `MainContent`. En concreto, este ContentPlaceHolder se encuentra dentro de la `mainContent` `<div>` elemento.

La figura 1 muestra `Default.aspx` cuando se ven a través de un explorador. La región con un círculo rojo es el marcado específico de la página correspondiente a `MainContent`.


[![La región en un círculo muestra el área actualmente personalizables según una página a página](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Figura 01**: la región de círculo muestra el área actualmente personalizables según una página por página ([haga clic aquí para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))


Imagine que además de la región que se muestra en la figura 1, también es necesario agregar elementos específicos de la página a la columna izquierda debajo de las lecciones y noticias secciones. Para ello, agregamos otro control ContentPlaceHolder a la página maestra. Para poder continuar, abra la `Site.master` página en Visual Web Developer principal y, a continuación, arrastre un control ContentPlaceHolder del cuadro de herramientas hasta el diseñador después de la sección de noticias. Establecer el ContentPlaceHolder `ID` a `LeftColumnContent`.


[![Agregar un Control ContentPlaceHolder a columna izquierda de la página maestra](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Figura 02**: agregar un ContentPlaceHolder Control a la columna izquierda de la página maestra ([haga clic aquí para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))


Con la adición de la `LeftColumnContent` ContentPlaceHolder a la página maestra, podemos definir el contenido de esta región según una página a página mediante la inclusión de contenido de un control en la página cuya `ContentPlaceHolderID` está establecido en `LeftColumnContent`. Analizaremos este proceso en el paso 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Paso 2: Definir el contenido de la nueva ContentPlaceHolder en las páginas de contenido

Al agregar una nueva página de contenido para el sitio Web, Visual Web Developer crea automáticamente un contenido de control en la página de cada ContentPlaceHolder en la página principal seleccionada. Una vez agregado un el `LeftColumnContent` ContentPlaceHolder a nuestra página maestra en el paso 1, nueva will de páginas ASP.NET ahora tienen tres controles de contenido.

Para ilustrar esto, agregue una nueva página de contenido en el directorio raíz denominado `MultipleContentPlaceHolders.aspx` que está enlazado a la `Site.master` página maestra. Visual Web Developer crea esta página con el siguiente marcado declarativo:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

Escriba algún contenido en el control de contenido que hacen referencia a la `MainContent` ContentPlaceHolders (`Content2`). A continuación, agregue el siguiente marcado para la `Content3` control de contenido (que hace referencia el `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Después de agregar este marcado, visite la página a través de un explorador. Como se muestra en la figura 3, el marcado se coloca en el `Content3` control de contenido se muestra en la columna izquierda, bajo la sección de noticias (con un círculo rojo). El marcado que se coloca en `Content2` se muestra en la parte derecha de la página (dentro de un círculo en azul).


[![La columna izquierda incluye ahora específica de la página contenido debajo de la sección de noticias](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Figura 03**: la izquierda columna ahora incluye específica de la página contenido debajo de la sección noticias ([haga clic aquí para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definir el contenido existentes en páginas de contenido

Crear una nueva página de contenido automáticamente incorpora el control ContentPlaceHolder que hemos agregado en el paso 1. Pero nuestros dos páginas de contenido existentes - `About.aspx` y `Default.aspx` -no tiene un control de contenido para el `LeftColumnContent` ContentPlaceHolder. Para especificar el contenido para este ContentPlaceHolder en estas dos páginas existentes, es necesario agregar un control de contenido nosotros mismos.

A diferencia de la mayoría de los controles Web de ASP.NET, el cuadro de herramientas de Visual Web Developer no incluye un elemento de control de contenido. Se puede escribir manualmente en el marcado declarativo del control de contenido en la vista del origen, pero un enfoque más rápido y sencillo es usar la vista de diseño. Abra la `About.aspx` página y cambie a la vista de diseño. Como se muestra en la figura 4, la `LeftColumnContent` ContentPlaceHolder aparece en la vista de diseño; si pasa el mouse sobre él, lee el título que se muestra: "LeftColumnContent (maestra)." La inclusión de "Master" en el título indica que no hay ningún control de contenido definido en la página para esta ContentPlaceHolder. Si existe un control de contenido para el control ContentPlaceHolder, como en el caso de `MainContent`, leerá el título: "*ContentPlaceHolderID* (personalizada)."

Para agregar un control de contenido para el `LeftColumnContent` ContentPlaceHolder a `About.aspx`, expanda la etiqueta inteligente del ContentPlaceHolder y haga clic en el vínculo Crear contenido personalizado.


[![La vista de diseño para About.aspx muestra el LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Figura 04**: la vista de diseño de `About.aspx` muestra la `LeftColumnContent` ContentPlaceHolder ([haga clic aquí para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))


Al hacer clic en el vínculo Crear contenido personalizado genera necesaria de contenido de control en la página y establece su `ContentPlaceHolderID` propiedad para el ContentPlaceHolder `ID`. Por ejemplo, al hacer clic en el vínculo Crear contenido personalizado para `LeftColumnContent` región en `About.aspx` agrega el siguiente marcado declarativo para la página:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Omisión de controles de contenido

ASP.NET no requiere que todas las páginas de contenido incluyen controles de contenido para cada ContentPlaceHolder definido en la página maestra. Si se omite un control de contenido, el motor ASP.NET utiliza el formato definido en el control ContentPlaceHolder en la página maestra. Esta marca se conoce como el ContentPlaceHolder *contenido predeterminado* y es útil en escenarios donde el contenido de algunos región es común para la mayoría de las páginas, pero debe personalizarse para un pequeño número de páginas. Paso 3 explora contenido predeterminado si se especifica en la página maestra.

Actualmente, `Default.aspx` contiene dos controles de contenido para el `head` y `MainContent` ContentPlaceHolders; no tiene un control de contenido para `LeftColumnContent`. Por lo tanto, cuando `Default.aspx` se representa el `LeftColumnContent` usan el contenido de forma predeterminada del ContentPlaceHolder. Dado que tenemos aún definir cualquier contenido predeterminado para este ContentPlaceHolder, el efecto neto es que no se emite ningún marcado para esta región. Para comprobar este comportamiento, visite `Default.aspx` a través de un explorador. Como se muestra en la figura 5, no se emite ningún tipo de marcado en la columna izquierda, bajo la sección de noticias.


[![No se representa ningún contenido para el LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Figura 05**: sin contenido se represente para el `LeftColumnContent` ContentPlaceHolder ([haga clic aquí para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Paso 3: Especificar el contenido de forma predeterminada en la página maestra

Algunos diseños de sitio Web incluyen un área cuyo contenido es el mismo para todas las páginas en el sitio salvo algunas excepciones de uno o dos. Considere la posibilidad de un sitio Web que es compatible con cuentas de usuario. Este tipo de sitio requiere una página de inicio de sesión que los visitantes pueden escribir sus credenciales para iniciar sesión en el sitio. Para acelerar el proceso de inicio de sesión, los diseñadores de sitios Web pueden ser cuadros de texto Nombre de usuario y contraseña en la esquina superior izquierda de cada página para permitir que los usuarios inicien sesión sin tener que visitar explícitamente la página de inicio de sesión. Aunque estos cuadros de texto Nombre de usuario y la contraseña son útiles en la mayoría de las páginas, son redundantes en la página de inicio de sesión, que ya contiene cuadros de texto para las credenciales del usuario.

Para implementar este diseño, podría crear un control ContentPlaceHolder en la esquina superior izquierda de la página maestra. Cada página que es necesario para mostrar los cuadros de texto Nombre de usuario y contraseña en su esquina superior izquierda podría crear un control de contenido para este ContentPlaceHolder y agregar la interfaz necesaria. La página de inicio de sesión, por otro lado, podría omitir o agregar un control de contenido para este ContentPlaceHolder o crearía un contenido de control con ningún tipo de marcado definida. El inconveniente de este enfoque es que se tiene que recordar agregar los cuadros de texto Nombre de usuario y una contraseña para cada página que se agrega al sitio (excepto la página de inicio de sesión). Esto es buscar problemas. Estamos menos posibilidades de olvidarse agregar estos cuadros de texto a una página o dos o, incluso peor, tengamos que implementa la interfaz correctamente (es posible agregar un solo cuadro de texto en lugar de dos).

Una solución mejor es definir los cuadros de texto Nombre de usuario y una contraseña como contenido del ContentPlaceHolder predeterminado. Al hacerlo, únicamente se necesita invalidar este contenido predeterminado en esas pocas páginas que no se muestran los cuadros de texto Nombre de usuario y contraseña (inicio de sesión de la página, por ejemplo). Para ilustrar especificar contenido predeterminado para un control ContentPlaceHolder, vamos a implementar el escenario que acabamos de describir.

> [!NOTE]
> El resto de este tutorial actualiza el sitio Web para incluir una interfaz de inicio de sesión en la columna izquierda para todas las páginas, pero la página de inicio de sesión. Sin embargo, en este tutorial no se examina cómo configurar el sitio Web para admitir las cuentas de usuario. Para obtener más información acerca de este tema, consulte mi [autenticación de formularios, autorización, cuentas de usuario y las funciones](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) tutoriales.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Agregar un ContentPlaceHolder y especificar su contenido predeterminado

Abra la `Site.master` página principal y agregue el siguiente marcado para la columna izquierda entre la `DateDisplay` sección etiqueta y lecciones:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Después de agregar esta marca de la vista de diseño de su página principal debe ser similar a la figura 6.


[![La página principal incluye un Control de inicio de sesión](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Figura 06**: la página principal incluye un Control de inicio de sesión ([haga clic aquí para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))


Este ContentPlaceHolder, `QuickLoginUI`, tiene un control de inicio de sesión Web como su contenido de forma predeterminada. El control de inicio de sesión muestra una interfaz de usuario que solicita al usuario su nombre de usuario y contraseña junto con un botón de inicio de sesión. Al hacer clic en el botón Iniciar sesión, el control de inicio de sesión internamente valida las credenciales del usuario con la API de pertenencia. Para utilizar este control de inicio de sesión en la práctica, a continuación, debe configurar su sitio para utilizar la suscripción. En este tema queda fuera del ámbito de este tutorial; hacer referencia a mi [autenticación de formularios, autorización, cuentas de usuario y las funciones](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) tutoriales para obtener más información sobre la creación de una aplicación web que admite las cuentas de usuario.

Puede personalizar el comportamiento o la apariencia del control de inicio de sesión. He configurado dos de sus propiedades: `TitleText` y `FailureAction`. La `TitleText` valor de la propiedad cuyo valor predeterminado es "Inicio de sesión", se muestra en la parte superior de la interfaz de usuario del control. He establecer esta propiedad para que muestre el texto "Iniciar sesión" como un `<h3>` elemento. El `FailureAction` propiedad indica qué hacer si las credenciales del usuario no son válidas. El valor predeterminado es un valor de `Refresh`, que deja al usuario en la misma página y muestra un mensaje de error en el control de inicio de sesión. He cambiado a `RedirectToLoginPage`, que envía el usuario a la página de inicio de sesión en el caso de credenciales no válidas. Prefiero enviar al usuario a la página de inicio de sesión cuando un usuario intenta iniciar sesión desde otra página, pero se produce un error, dado que la página de inicio de sesión puede contener instrucciones adicionales y opciones que no se adaptaría fácilmente a la columna izquierda. Por ejemplo, la página de inicio de sesión podría incluir opciones para recuperar una contraseña olvidada o crear una nueva cuenta.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Creación de la página de inicio de sesión y reemplazar el contenido predeterminado

Con la página principal completa, el siguiente paso es crear la página de inicio de sesión. Agregar una página ASP.NET al directorio raíz de su sitio denominado `Login.aspx`, enlace a la `Site.master` página maestra. Si lo hace, se creará una página con cuatro controles de contenido, uno para cada uno de los ContentPlaceHolders definidos en `Site.master`.

Agregar un control de inicio de sesión para el `MainContent` control de contenido. Del mismo modo, no dude en agregar cualquier contenido en el `LeftColumnContent` región. Sin embargo, asegúrese de dejar el control de contenido para el `QuickLoginUI` ContentPlaceHolder vacío. Esto garantizará que el control no aparece en la columna izquierda de la página de inicio de sesión de inicio de sesión.

Después de definir el contenido de la `MainContent` y `LeftColumnContent` regiones, el marcado declarativo de la página de inicio de sesión debe ser similar al siguiente:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

La figura 7 muestra esta página cuando se ve mediante un explorador. Puesto que esta página especifica un control de contenido para el `QuickLoginUI` ContentPlaceHolder, reemplaza el contenido predeterminado especificado en la página maestra. El efecto neto es que el control de inicio de sesión se muestra en la vista (consulte la figura 6) no se representa en esta página de diseño de la página maestra.


[![La página de inicio de sesión Represses predeterminado contenido de QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Figura 07**: la página de inicio de sesión Represses la `QuickLoginUI` predeterminado contenido del ContentPlaceHolder ([haga clic aquí para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Utilizando el contenido de forma predeterminada en las páginas nuevas

Queremos mostrar el control de inicio de sesión en la columna izquierda para todas las páginas excepto la página de inicio de sesión. Para lograr esto, todas las páginas de contenido excepto la página de inicio de sesión deben omitir un control de contenido para el `QuickLoginUI` ContentPlaceHolder. Mediante la omisión de un control de contenido, contenido del ContentPlaceHolder predeterminado se usará en su lugar.

Nuestras páginas de contenido existentes - `Default.aspx`, `About.aspx`, y `MultipleContentPlaceHolders.aspx` -no incluyen un control de contenido para `QuickLoginUI` porque se crearon antes de que se agreguen ese control ContentPlaceHolder a la página maestra. Por lo tanto, estas páginas existentes no es necesario actualizar. Sin embargo, páginas que se agreguen al sitio Web incluyen un control de contenido para el `QuickLoginUI` ContentPlaceHolder, de forma predeterminada. Por lo tanto, tenemos que no olvide quitar estos controles de contenido cada vez que se agregue una nueva página de contenido (a menos que queremos invalidar contenido de forma predeterminada del ContentPlaceHolder, como en el caso de la página de inicio de sesión).

Para quitar el control de contenido, puede manualmente eliminar su marcado declarativo de la vista del origen o, en la vista Diseño, elija el valor predeterminado para el vínculo de contenido del principal de la etiqueta inteligente. Cualquier enfoque quita el control de contenido de la página y genera el mismo efecto de red.

La figura 8 muestra `Default.aspx` cuando se ven a través de un explorador. Recuerde que `Default.aspx` solo tiene dos controles de contenido especificados en el marcado declarativo: uno para `head` y otro para `MainContent`. Como resultado, el valor predeterminado de contenido para el `LeftColumnContent` y `QuickLoginUI` ContentPlaceHolders se muestran.


[![Se muestran el contenido predeterminado para el LeftColumnContent y QuickLoginUI ContentPlaceHolders](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Figura 08**: The Default Content para el `LeftColumnContent` y `QuickLoginUI` se muestran ContentPlaceHolders ([haga clic aquí para ver la imagen a tamaño completo](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))


## <a name="summary"></a>Resumen

El modelo de página maestra de ASP.NET permite un número arbitrario de ContentPlaceHolders en la página maestra. ¿Qué es más, ContentPlaceHolders incluyen contenido predeterminado, que se genera en el caso de que no hay ningún correspondiente contenido de control en la página de contenido. En este tutorial se ha explicado cómo incluir controles ContentPlaceHolder adicionales en la página maestra y cómo definir controles de contenido para estos nuevos ContentPlaceHolders en las páginas ASP.NET nuevas y existentes. También explicamos especificar predeterminado contenidos en un ContentPlaceHolder, lo que resulta útil en escenarios donde solo una minoría de necesidades de páginas para personalizar el en caso contrario estandarizados contenido dentro de una región determinada.

En el siguiente tutorial examinaremos el `head` ContentPlaceHolder con más detalle, ver cómo definir mediante declaración y mediante programación una página a página según el título, etiquetas meta y otros encabezados HTML.

Feliz programación.

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Suchi Banerjee. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-a-site-wide-layout-using-master-pages-cs.md)
> [Siguiente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
