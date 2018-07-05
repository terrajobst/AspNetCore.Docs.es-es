---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Varios ContentPlaceHolders y contenido predeterminado (VB) | Microsoft Docs
author: rick-anderson
description: Examina cómo agregar varios marcadores de posición de contenido a una página maestra, así como la especificación de contenido predeterminado de los marcadores de posición de contenido.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: 5425d59ac1ccec46617400601655ae72f860cab1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395119"
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>Varios ContentPlaceHolders y contenido predeterminado (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) o [descargar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Examina cómo agregar varios marcadores de posición de contenido a una página maestra, así como la especificación de contenido predeterminado de los marcadores de posición de contenido.


## <a name="introduction"></a>Introducción

En el tutorial anterior hemos visto cómo las páginas maestras permiten a los desarrolladores ASP.NET para crear un diseño coherente de todo el sitio. Páginas maestras definen el marcado que es común a todas sus páginas de contenido y las regiones que son personalizables según una página por página. En el tutorial anterior, creamos una simple página maestra (`Site.master`) y dos páginas de contenido (`Default.aspx` y `About.aspx`). Nuestra página maestra estaba formada por dos ContentPlaceHolders denominados `head` y `MainContent`, que se encuentra en la `<head>` elemento y el formulario Web Forms, respectivamente. Mientras que las páginas de contenido tenían dos controles de contenido, sólo especificamos marcado por el correspondiente a `MainContent`.

Tal como se evidencia por los dos controles ContentPlaceHolder en `Site.master`, una página principal puede contener varios ContentPlaceHolders. Además, la página maestra puede especificar el marcado predeterminado para los controles ContentPlaceHolder. Una página de contenido, a continuación, puede, opcionalmente, especificar su propio marcado o usar el marcado de forma predeterminada. En este tutorial se usar varios controles de contenido en la página maestra y veamos cómo definir el marcado predeterminado en los controles ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Paso 1: Agregar controles ContentPlaceHolder adicionales a la página maestra

Muchos diseños de sitio Web contienen varias áreas en la pantalla que se personalizan según una página por página. `Site.master`, la página maestra que se creó en el tutorial anterior, contiene sólo un ContentPlaceHolder dentro del formulario Web denominado `MainContent`. En concreto, se encuentra dentro de este ContentPlaceHolder el `mainContent` `<div>` elemento.

La figura 1 muestra `Default.aspx` cuando se ve mediante un explorador. La región con un círculo rojo es el marcado específico de la página correspondiente a `MainContent`.


[![La región en un círculo muestra el área actualmente personalizables según una página por página](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Figura 01**: la región en círculo muestra el área actualmente personalizables según una página por página ([haga clic aquí para ver imagen en tamaño completo](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


Imagine que además de la región que se muestra en la figura 1, también es necesario agregar elementos específicos de la página a la columna izquierda debajo de las lecciones y noticias secciones. Para ello, agregamos otro control ContentPlaceHolder a la página maestra. Para poder continuar, abra el `Site.master` página en Visual Web Developer principal y, a continuación, arrastre un control ContentPlaceHolder desde el cuadro de herramientas hasta el diseñador después de la sección de noticias. Establecer el ContentPlaceHolder `ID` a `LeftColumnContent`.


[![Agregar un Control ContentPlaceHolder a la columna izquierda de la página maestra](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Figura 02**: agregar un ContentPlaceHolder Control a la columna izquierda de la página maestra ([haga clic aquí para ver imagen en tamaño completo](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


Con la adición de la `LeftColumnContent` ContentPlaceHolder a la página maestra, podemos definir el contenido de esta región según una página a página mediante la inclusión de contenido de un control en la página cuya `ContentPlaceHolderID` está establecido en `LeftColumnContent`. Analizaremos este proceso en el paso 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Paso 2: Definición de contenido para el control ContentPlaceHolder nuevas en las páginas de contenido

Al agregar una nueva página de contenido al sitio Web, Visual Web Developer crea automáticamente el contenido de un control en la página para cada control ContentPlaceHolder en la página principal seleccionada. Una vez agregado un los `LeftColumnContent` ContentPlaceHolder a nuestra página maestra en el paso 1, nuevo voluntad de páginas ASP.NET ahora tiene tres controles de contenido.

Para ilustrar esto, agregue una nueva página de contenido en el directorio raíz denominado `MultipleContentPlaceHolders.aspx` que está enlazado a la `Site.master` página maestra. Visual Web Developer crea esta página con el siguiente marcado declarativo:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Escriba algún contenido en el control de contenido que hacen referencia a la `MainContent` ContentPlaceHolders (`Content2`). A continuación, agregue el siguiente marcado para la `Content3` control de contenido (que hace referencia a la `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Después de agregar este marcado, visite la página a través de un explorador. Como se muestra en la figura 3, el marcado se coloca en el `Content3` control de contenido se muestra en la columna izquierda, bajo la sección de noticias (rodeada en rojo). El marcado que se coloca en `Content2` se muestra en la parte derecha de la página (marcadas con un círculo azul).


[![La columna izquierda ahora incluye el contenido específico de página debajo de la sección de noticias](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Figura 03**: The izquierda columna ahora incluye específica de la página contenido bajo la sección de noticias ([haga clic aquí para ver imagen en tamaño completo](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definición de contenido en las páginas de contenido existentes

Crear automáticamente una nueva página de contenido incorpora el control ContentPlaceHolder, que hemos agregado en el paso 1. Pero nuestras dos páginas de contenido existentes - `About.aspx` y `Default.aspx` -no tiene un control de contenido para el `LeftColumnContent` ContentPlaceHolder. Para especificar el contenido para este control ContentPlaceHolder en estas dos páginas existentes, necesitamos agregar un control de contenido nosotros mismos.

A diferencia de la mayoría de los controles Web de ASP.NET, el cuadro de herramientas de Visual Web Developer no incluye un elemento de control de contenido. Además podemos escribir manualmente en el marcado declarativo del control de contenido en la vista del origen, pero un enfoque más fácil y rápido consiste en usar la vista de diseño. Abra el `About.aspx` página y cambie a la vista de diseño. Como se muestra en la figura 4, el `LeftColumnContent` ContentPlaceHolder aparece en la vista de diseño; si pasa el mouse sobre él, lee el título que se muestra: "LeftColumnContent (maestra)." La inclusión de "Master" en el título indica que no hay ningún control de contenido definido en la página para este ContentPlaceHolder. Si existe un control de contenido para el control ContentPlaceHolder, como se muestra en el caso de `MainContent`, leerá el título: "*ContentPlaceHolderID* (personalizada)."

Para agregar un control de contenido para el `LeftColumnContent` ContentPlaceHolder a `About.aspx`, expanda la etiqueta inteligente del ContentPlaceHolder y haga clic en el vínculo Crear contenido personalizado.


[![La vista de diseño para About.aspx muestra el LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Figura 04**: la vista de diseño para `About.aspx` muestra el `LeftColumnContent` ContentPlaceHolder ([haga clic aquí para ver imagen en tamaño completo](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


Al hacer clic en el vínculo Crear contenido personalizado genera el necesario de contenido de control en la página y establece su `ContentPlaceHolderID` propiedad en el ContentPlaceHolder `ID`. Por ejemplo, al hacer clic en el vínculo Crear contenido personalizado para `LeftColumnContent` región en `About.aspx` agrega el siguiente marcado declarativo a la página:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Si se omite los controles de contenido

ASP.NET no requiere que todas las páginas de contenido incluyen controles de contenido para cada ContentPlaceHolder definido en la página maestra. Si se omite un control de contenido, el motor ASP.NET utiliza el marcado definido en el ContentPlaceHolder en la página maestra. Este marcado se conoce como el ContentPlaceHolder *contenido predeterminado* y es útil en escenarios donde el contenido de algunas regiones es común para la mayoría de las páginas, pero debe personalizarse para un pequeño número de páginas. Paso 3 explora especificando contenido predeterminado en la página maestra.

Actualmente, `Default.aspx` contiene dos controles de contenido para el `head` y `MainContent` ContentPlaceHolders; no tiene un control de contenido para `LeftColumnContent`. Por lo tanto, cuando `Default.aspx` se representa el `LeftColumnContent` usan el contenido de forma predeterminada del ContentPlaceHolder. Dado que aún es necesario definir ningún contenido predeterminado para este control ContentPlaceHolder, el efecto neto es que no se emite ningún marcado para esta región. Para comprobar este comportamiento, visite `Default.aspx` a través de un explorador. Como se muestra en la figura 5, no hay ningún marcado se genera en la columna izquierda debajo de la sección de noticias.


[![No hay contenido se represente para el LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Figura 05**: sin contenido se represente para el `LeftColumnContent` ContentPlaceHolder ([haga clic aquí para ver imagen en tamaño completo](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Paso 3: Especificar el contenido de forma predeterminada en la página maestra

Algunos diseños de sitios Web incluyen un área cuyo contenido es el mismo para todas las páginas del sitio, excepto una o dos excepciones. Considere la posibilidad de un sitio Web que admite las cuentas de usuario. Este tipo de sitio requiere una página de inicio de sesión donde los visitantes pueden escribir sus credenciales para iniciar sesión en el sitio. Para acelerar el proceso de inicio de sesión, los diseñadores del sitio Web pueden incluir los cuadros de texto Nombre de usuario y contraseña en la esquina superior izquierda de cada página para permitir que los usuarios inicien sesión sin tener que visite la página de inicio de sesión de forma explícita. Aunque estos cuadros de texto Nombre de usuario y contraseña son útiles en la mayoría de las páginas, son redundantes en la página de inicio de sesión, que ya contiene cuadros de texto para las credenciales del usuario.

Para implementar este diseño, puede crear un control ContentPlaceHolder en la esquina superior izquierda de la página maestra. Cada página que es necesario para mostrar los cuadros de texto Nombre de usuario y contraseña en su esquina superior izquierda podría crear un control de contenido para este ContentPlaceHolder y agregue la interfaz necesaria. La página de inicio de sesión, por otro lado, ya sea omitiría la adición de un control de contenido para este ContentPlaceHolder o crearía un contenido de control con ningún marcado definido. La desventaja de este enfoque es que tenemos que no olvide agregar los cuadros de texto Nombre de usuario y contraseña para cada página que se agrega al sitio (excepto la página de inicio de sesión). Esto es buscar problemas. Es probables que se olvide de agregar estos cuadros de texto a una página o dos o, peor aún, podríamos no implementamos la interfaz correctamente (quizás agregando un solo cuadro de texto en lugar de dos).

Una solución mejor es definir los cuadros de texto Nombre de usuario y contraseña como contenido de forma predeterminada del ContentPlaceHolder. Al hacerlo, sólo se necesita reemplazar este contenido de forma predeterminada en esas pocas páginas que no se muestran los cuadros de texto Nombre de usuario y contraseña (inicio de sesión de la página, por ejemplo). Para ilustrar especificando contenido predeterminado para un control ContentPlaceHolder, vamos a implementar el escenario que acabamos de describir.

> [!NOTE]
> El resto de este tutorial, actualiza nuestro sitio Web para incluir una interfaz de inicio de sesión en la columna izquierda para todas las páginas, pero la página de inicio de sesión. Sin embargo, este tutorial no examina cómo configurar el sitio Web para admitir cuentas de usuario. Para obtener más información sobre este tema, consulte mi [formularios de autenticación, autorización, las cuentas de usuario y las funciones](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) tutoriales.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Agregar un ContentPlaceHolder y especificar su contenido predeterminado

Abra el `Site.master` página principal y agregue el marcado siguiente a la columna izquierda entre la `DateDisplay` sección etiqueta y lecciones:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Después de agregar esta marca de la vista de diseño de su página principal debe ser similar a la figura 6.


[![La página principal incluye un Control de inicio de sesión](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Figura 06**: la página principal incluye un Control de inicio de sesión ([haga clic aquí para ver imagen en tamaño completo](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


Este control ContentPlaceHolder, `QuickLoginUI`, tiene un control de inicio de sesión Web como su contenido de forma predeterminada. El control de inicio de sesión muestra una interfaz de usuario que solicita al usuario su nombre de usuario y contraseña junto con un botón Iniciar sesión. Al hacer clic en el botón Iniciar sesión, el control de inicio de sesión internamente valida las credenciales del usuario con la API de pertenencia. Para usar este control de inicio de sesión en la práctica, a continuación, deberá configurar su sitio para utilizar la suscripción. En este tema queda fuera del ámbito de este tutorial, hacer referencia a mi [formularios de autenticación, autorización, las cuentas de usuario y las funciones](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) tutoriales para obtener más información sobre la creación de una aplicación web que admite las cuentas de usuario.

No dude en Personalizar el comportamiento o la apariencia del control de inicio de sesión. He configurado dos de sus propiedades: `TitleText` y `FailureAction`. El `TitleText` valor de propiedad cuyo valor predeterminado es "Iniciar sesión", se muestra en la parte superior de la interfaz de usuario del control. He establecer esta propiedad para que muestre el texto "Iniciar sesión" como un `<h3>` elemento. El `FailureAction` propiedad indica qué hacer si las credenciales del usuario no son válidas. El valor predeterminado es un valor de `Refresh`, que deja al usuario en la misma página y muestra un mensaje de error en el control de inicio de sesión. He cambiado a `RedirectToLoginPage`, que envía el usuario a la página de inicio de sesión en el caso de credenciales no válidas. Yo prefiero enviar al usuario a la página de inicio de sesión cuando un usuario intenta iniciar sesión desde otra página, pero se produce un error, dado que la página de inicio de sesión puede contener instrucciones adicionales y opciones que no podría encajar con facilidad con la columna izquierda. Por ejemplo, la página de inicio de sesión podría incluir opciones para recuperar una contraseña olvidada o para crear una nueva cuenta.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Creación de la página de inicio de sesión y reemplazando el contenido predeterminado

Con la página principal completa, el siguiente paso es crear la página de inicio de sesión. Agregar una página ASP.NET en el directorio raíz de su sitio denominado `Login.aspx`, enlazarlo a la `Site.master` página maestra. Si lo hace, creará una página con cuatro controles de contenido, uno para cada uno de los ContentPlaceHolders definido en `Site.master`.

Agregar un control de inicio de sesión para el `MainContent` control de contenido. Del mismo modo, puede agregar contenido a la `LeftColumnContent` región. Sin embargo, asegúrese de dejar el control de contenido para el `QuickLoginUI` ContentPlaceHolder vacío. Esto garantizará que el control no aparece en la columna izquierda de la página de inicio de sesión de inicio de sesión.

Después de definir el contenido de la `MainContent` y `LeftColumnContent` regiones, el marcado declarativo de la página de inicio de sesión debe ser similar al siguiente:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Figura 7 muestra esta página cuando se ve mediante un explorador. Puesto que esta página especifica un control de contenido para el `QuickLoginUI` ContentPlaceHolder, reemplaza el contenido predeterminado especificado en la página maestra. El efecto neto es que el control de inicio de sesión se muestra en la vista (consulte la figura 6) no se representa en esta página de diseño de la página maestra.


[![La página de inicio de sesión Represses contenido de forma predeterminada de QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Figura 07**: la página de inicio de sesión Represses el `QuickLoginUI` predeterminado contenido del ContentPlaceHolder ([haga clic aquí para ver imagen en tamaño completo](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Utilizando el contenido de forma predeterminada en las páginas nuevas

Queremos que aparezca el control de inicio de sesión en la columna izquierda para todas las páginas, excepto la página de inicio de sesión. Para lograr esto, todas las páginas de contenido, excepto la página de inicio de sesión deben omitir un control de contenido para el `QuickLoginUI` ContentPlaceHolder. Si se omite un control de contenido, contenido del ContentPlaceHolder predeterminado se usará en su lugar.

Nuestras páginas de contenido existentes - `Default.aspx`, `About.aspx`, y `MultipleContentPlaceHolders.aspx` -no incluyen un control de contenido para `QuickLoginUI` porque se crearon antes de agregar ese control ContentPlaceHolder a la página maestra. Por lo tanto, estas páginas existentes no es necesario actualizar. Sin embargo, páginas que se agreguen al sitio Web incluyen un control de contenido para el `QuickLoginUI` ContentPlaceHolder, de forma predeterminada. Por lo tanto, tenemos que no olvide quitar estos controles de contenido cada vez que se agregue una nueva página de contenido (a menos que deseamos reemplazar el contenido de forma predeterminada del ContentPlaceHolder, como en el caso de la página de inicio de sesión).

Para quitar el control de contenido, puede manualmente eliminar su marcado declarativo de la vista del origen o, en la vista Diseño, elija el valor predeterminado para el vínculo de contenido del patrón de su etiqueta inteligente. Cualquiera de los enfoques quita el control de contenido de la página y genera el mismo efecto de red.

La figura 8 muestra `Default.aspx` cuando se ve mediante un explorador. Recuerde que `Default.aspx` solo tiene dos controles de contenido especificados en el marcado declarativo: uno para `head` y otro para `MainContent`. Como resultado, el valor predeterminado de contenido para el `LeftColumnContent` y `QuickLoginUI` ContentPlaceHolders se muestran.


[![Se muestran el contenido predeterminado para el LeftColumnContent y QuickLoginUI ContentPlaceHolders](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Figura 08**: The Default contenido para el `LeftColumnContent` y `QuickLoginUI` aparecen ContentPlaceHolders ([haga clic aquí para ver imagen en tamaño completo](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>Resumen

El modelo de página maestra ASP.NET permite un número arbitrario de ContentPlaceHolders en la página maestra. ¿Qué es más, ContentPlaceHolders incluyen contenido predeterminado, que se genera en el caso de que no hay ningún correspondiente control en la página de contenido de contenido. En este tutorial hemos visto cómo incluir controles ContentPlaceHolder adicionales en la página maestra y cómo definir controles de contenido de estos nuevos ContentPlaceHolders en las páginas ASP.NET nuevas y existentes. También hemos analizado especificar predeterminados contenidos en un ContentPlaceHolder, que es útil en escenarios donde solo una minoría de las necesidades de las páginas para personalizar el en caso contrario estandarizado contenido dentro de una región determinada.

En el siguiente tutorial examinaremos el `head` ContentPlaceHolder con más detalle, viendo cómo mediante declaración y mediante programación, definir el título, etiquetas meta y otros encabezados HTML en la página por página.

Feliz programación.

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [ *Sams Teach usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Suchi Banerjee. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-a-site-wide-layout-using-master-pages-vb.md)
> [Siguiente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
