---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: "Las direcciones URL de páginas maestras (VB) | Documentos de Microsoft"
author: rick-anderson
description: "Aborda cómo pueden interrumpir las direcciones URL en la página maestra porque el archivo de página maestra que se está en un directorio diferente relativo a la página de contenido. Examina el reajuste..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 8aa0ed2fbf385e4b8dbb7e7a3bdb152f1e016e67
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="urls-in-master-pages-vb"></a>Direcciones URL de páginas maestras (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Aborda cómo pueden interrumpir las direcciones URL en la página maestra porque el archivo de página maestra que se está en un directorio diferente relativo a la página de contenido. Se examinan reajustar las direcciones URL a través de ~ en la sintaxis declarativa y usar ResolveUrl y ResolveClientUrl mediante programación. (Mire también


## <a name="introduction"></a>Introducción

En todos los ejemplos hemos visto hasta ahora, que las páginas maestras y de contenido se han encontrado en la misma carpeta (carpeta de raíz del sitio Web). Pero no hay ninguna razón por qué las páginas maestras y de contenido deben estar en la misma carpeta. Por supuesto, puede crear páginas de contenido en las subcarpetas. De forma similar, puede crear un `~/MasterPages/` carpeta donde colocar páginas maestras de su sitio.

Un problema potencial con la colocación de las páginas maestras y de contenido en carpetas diferentes implica interrumpe las direcciones URL. Si la página maestra contiene direcciones URL relativas en hipervínculos, imágenes u otros elementos, el vínculo será válido para las páginas de contenido que residen en una carpeta diferente. En este tutorial se examina el origen de este problema, así como soluciones alternativas.

## <a name="the-problem-with-relative-urls"></a>El problema con direcciones URL relativas

Se dice que una dirección URL en una página web sea un *dirección URL relativa* si es la ubicación del recurso que apunta en relación con la ubicación de la página web en la estructura de carpetas del sitio Web. Cualquier dirección URL que no se inicia con una barra diagonal inicial (`/`) o un protocolo (como `http://`) es relativa debido a que se resuelven mediante el explorador basándose en la ubicación de la página web que contiene la dirección URL.

Por ejemplo, nuestro sitio Web tiene un `~/Images/` carpeta con un único archivo de imagen, `PoweredByASPNET.gif`. El archivo de página maestra `Site.master` tiene un `<img>` elemento en el `footerContent` región con el marcado siguiente:


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

El `src` atributo valor en el `<img>` elemento es una dirección URL relativa porque no se inicia con `/` o `http://`. En resumen, la `src` valor de atributo indica al explorador que busque en el `Images` subcarpeta en un archivo denominado `PoweredByASPNET.gif`.

Al visitar una página de contenido, el marcado anterior se envía directamente al explorador. Dedique unos minutos a visitar `About.aspx` y ver el código HTML que se envía al explorador. Encontrará que el marcado de la mismo exacto en la página maestra se envía al explorador.


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

Si la página de contenido se encuentra en la carpeta raíz (como es `About.aspx`) que todo funciona según lo previsto porque no hay un `Images` subcarpeta con respecto a la carpeta raíz. Sin embargo, las cosas rompen hacia abajo si la página de contenido se encuentra en una carpeta diferente a la página maestra. Para ilustrar esto, cree una subcarpeta denominada `Admin`. A continuación, agregue una página de contenido denominada `Default.aspx` a la `Admin` carpeta, asegurándose de enlazar la nueva página a la `Site.master` página maestra.

> [!NOTE]
> En el [ *especificar el título, etiquetas Meta y otros encabezados de HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial hemos creado una clase de página base personalizada denominada `BasePage` que establece automáticamente el título de la página de contenido (si lo no asignó explícitamente). No olvide que la clase de código subyacente de la página recién creado se derive de `BasePage` para que puede utilizar esta funcionalidad.


Después de haber creado esta página de contenido, el Explorador de soluciones debe ser similar a la figura 1.


![Una carpeta nueva y la página de ASP.NET que se agregaron al proyecto](urls-in-master-pages-vb/_static/image1.png)

**Figura 01**: una carpeta nueva y la página de ASP.NET que se agregaron al proyecto


A continuación, actualice el `Web.sitemap` que incluya un nuevo archivo `<siteMapNode>` entrada en esta lección. El siguiente XML muestra la sección completa `Web.sitemap` marcado, que ahora incluye la adición de una tercera `<siteMapNode>` elemento.


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

Recién creado `Default.aspx` página debe tener cuatro controles de contenido correspondiente a las cuatro ContentPlaceHolders en `Site.master`. Agregue el texto para el control de contenido que hacen referencia a la `MainContent` ContentPlaceHolder y, a continuación, visite la página a través de un explorador. Como se muestra en la figura 2, el explorador no se puede encontrar el `PoweredByASPNET.gif` archivo de imagen. ¿Qué está sucediendo aquí?

El `~/Admin/Default.aspx` página de contenido se envía el mismo código de HTML la `footerContent` región como ocurría en la `About.aspx` página:


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Dado que la `<img>` del elemento `src` atributo es una dirección URL relativa, el explorador intenta buscar una `Images` carpeta con respecto a la ubicación de la carpeta de la página web. En otras palabras, el explorador está buscando el archivo de imagen `Admin/Images/PoweredByASPNET.gif`.


[![No se puede encontrar el archivo de imagen PoweredByASPNET.gif](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Figura 02**: el `PoweredByASPNET.gif` imagen no se encuentra el archivo ([haga clic aquí para ver la imagen a tamaño completo](urls-in-master-pages-vb/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Reemplazar las direcciones URL relativas con las direcciones URL absolutas

Lo contrario de una dirección URL relativa es un *dirección URL absoluta*, que es uno que se inicia con una barra diagonal (`/`) o un protocolo como `http://`. Dado que una dirección URL absoluta especifica la ubicación de un recurso desde un punto fijo conocido, la misma dirección URL absoluta es válida en cualquier página web, independientemente de la ubicación de la página web en la estructura de carpetas del sitio Web.

Para solucionar la imagen rota se muestra en la figura 2, deberá actualizar el `<img>` del elemento `src` atributo para que utilice una dirección URL absoluta en lugar de una relativa. Para determinar la dirección URL absoluta correcta, visite una de las páginas web en su sitio Web y examine la barra de direcciones. Tal y como se muestra en la barra de direcciones en la figura 2, la ruta de acceso completa a la aplicación web es `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. Por lo tanto, podríamos actualizar el `<img>` del elemento `src` atribuir a cualquiera de las siguientes dos URL absolutas:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

Tómese un momento para actualizar la `<img>` del elemento `src` atributo a una dirección URL absoluta mediante uno de los formularios mostrados anteriormente y, a continuación, visite la `~/Admin/Default.aspx` página a través de un explorador. Esta vez el explorador correctamente buscar y mostrar el `PoweredByASPNET.gif` archivo de imagen (consulte la figura 3).


[![La imagen PoweredByASPNET.gif es ahora muestran](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Figura 03**: el `PoweredByASPNET.gif` imagen es ahora muestran ([haga clic aquí para ver la imagen a tamaño completo](urls-in-master-pages-vb/_static/image7.png))


Si bien codificar de forma rígida en la dirección URL absoluta funciona, asocia estrictamente el código HTML al servidor del sitio Web y la ubicación de carpeta, que puede variar. Mediante una dirección URL absoluta del formulario `http://localhost:3908/...` es complicado porque el número de puerto anterior localhost se selecciona automáticamente cada vez que se inicia el servidor integrado ASP.NET desarrollo Web de Visual Studio. De forma similar, la `http://localhost` parte solo es válida cuando se prueba localmente. Una vez que el código se implementa en un servidor de producción, la dirección URL base cambia a otra cosa, como `http://www.yourserver.com`. La dirección URL absoluta con el formato `/ASPNET_MasterPages_Tutorial_04_VB/...` también se resiente de la fragilidad mismo porque a menudo esta ruta de acceso de la aplicación difiere entre los servidores de desarrollo y producción.

Las buenas noticias son que ofrece un método para generar una dirección URL relativa válida en tiempo de ejecución ASP.NET.

## <a name="usingandresolveclienturl"></a>Mediante`~`y`ResolveClientUrl`

En su lugar de codificar de forma rígida una dirección URL absoluta, ASP.NET permite a los desarrolladores de páginas usar la tilde (`~`) para indicar la raíz de la aplicación web. Por ejemplo, anteriormente en este tutorial utiliza la notación `~/Admin/Default.aspx` en el texto que se va a hacer referencia a la `Default.aspx` página en el `Admin` carpeta. El `~` indica que el `Admin` carpeta es una subcarpeta de la raíz de la aplicación web.

El `Control` la clase [ `ResolveClientUrl` método](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) toma una dirección URL y modifica a una dirección URL relativa adecuada para la página web en el que reside el control. Por ejemplo, al llamar a `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` de `About.aspx` devuelve `Images/PoweredByASPNET.gif`. Llamarlo desde `~/Admin/Default.aspx`, sin embargo, se devuelve `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Dado que todos los controles de servidor ASP.NET que se derivan de la `Control` (clase), todos los controles de servidor tienen acceso a la `ResolveClientUrl` método. Incluso la `Page` clase se deriva de la `Control` (clase), lo que significa que puede usar este método directamente desde las clases de código subyacente de las páginas ASP.NET.


### <a name="usingin-the-declarative-markup"></a>Usar`~`en el marcado declarativo

Varios controles Web de ASP.NET incluyen propiedades relacionadas con la dirección URL: el control de hipervínculo tiene un `NavigateUrl` propiedad; la imagen de control tiene un `ImageUrl` propiedad; y así sucesivamente. Cuando se representan, estos controles pasan los valores de propiedades relacionadas con la dirección URL que se `ResolveClientUrl`. Por lo tanto, si estas propiedades contienen un `~` para indicar la raíz de la aplicación web, se modificará la dirección URL a una URL relativa válida.

Tenga en cuenta que solo los controles de servidor ASP.NET transforman el `~` en sus propiedades relacionadas con la dirección URL. Si un `~` aparece en el marcado HTML estático, como `<img src="~/Images/PoweredByASPNET.gif" />`, el motor de ASP.NET envía el `~` al explorador junto con el resto del contenido HTML. El explorador se da por supuesto que el `~` forma parte de la dirección URL. Por ejemplo, si el explorador recibe el marcado `<img src="~/Images/PoweredByASPNET.gif" />` se supone que existe una subcarpeta denominada `~` con una subcarpeta `Images` que contiene el archivo de imagen `PoweredByASPNET.gif`.

Para corregir el marcado de la imagen en `Site.master`, reemplazar a las `<img>` elemento con un control de imagen Web de ASP.NET. Establecer el control de imagen Web `ID` a `PoweredByImage`, sus `ImageUrl` propiedad `~/Images/PoweredByASPNET.gif`y su `AlternateText` propiedad a "Con tecnología ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Después de realizar este cambio en la página maestra, volver a visitar la `~/Admin/Default.aspx` página de nuevo. Esta vez el `PoweredByASPNET.gif` archivo de imagen aparece en la página (consulte la figura 3). Cuando la Web de imagen de control es hacía que se usa el `ResolveClientUrl` método para resolver su `ImageUrl` valor de propiedad. En `~/Admin/Default.aspx` el `ImageUrl` se convierte en una dirección URL relativa apropiada, como el siguiente fragmento de código fuente HTML muestra:


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Además de su uso en Propiedades de control Web basado en la dirección URL, el `~` también puede usarse al llamar a la `Response.Redirect` y `Server.MapPath` métodos, entre otros. Además, el `ResolveClientUrl` método se puede invocar directamente desde un ASP.NET o marcado declarativo de la página principal, si es necesario, consulte [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)de entrada de blog [mediante `ResolveClientUrl` en marcado](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Corrección de la página maestra restante direcciones URL relativas

Además el `<img>` elemento en el `footerContent` que se acaba de corregido, la página maestra contiene una dirección URL relativa más que requiere atención. El `topContent` región incluye el vínculo "Master páginas tutoriales", que señala al `Default.aspx`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Dado que esta dirección URL es relativa, enviará al usuario la `Default.aspx` página de la carpeta de la página de contenido que están visitando. Para que este vínculo siempre apuntar a `Default.aspx` en la carpeta raíz que se deba reemplazar el `<a>` control de elemento con un sitio Web de hipervínculo, podemos utilizar la `~` notación.

Quitar el `<a>` marcado de elemento y agregar un control de hipervínculo en su lugar. Para establecer el hipervínculo `ID` a `lnkHome`, sus `NavigateUrl` propiedad `~/Default.aspx`y su `Text` propiedad a "Tutoriales de páginas maestra".


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

Ya está. En este momento, todas las direcciones URL en la página maestra se basan correctamente cuando se representa mediante una página de contenido, independientemente de qué carpetas de la página maestra y la página de contenido se encuentran en.

### <a name="automatic-url-resolution-in-theheadsection"></a>Resolución automática de dirección URL en la`<head>`sección

En el [ *crear una páginas de todo el sitio diseño utilizando Master* ](creating-a-site-wide-layout-using-master-pages-vb.md) tutorial agregamos un `<link>` a la `Styles.css` un archivo en el `<head>` región:


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Mientras el `<link>` del elemento `href` atributo es relativa, se convierte automáticamente en una ruta de acceso adecuado en tiempo de ejecución. Como se explicó en la [ *especificar el título, etiquetas Meta y otros encabezados de HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial, el `<head>` región es realmente un control de servidor, lo que permite modificar el contenido de sus controles internos cuando se represente.

Para comprobarlo, volver a visitar la `~/Admin/Default.aspx` página y ver el código HTML que se envía al explorador. Como se muestra en el siguiente fragmento, el `<link>` del elemento `href` atributo se ha modificado automáticamente a una dirección URL relativa adecuada `../Styles.css`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Resumen

Páginas maestras muy a menudo incluyen vínculos, imágenes y otros recursos externos que deben especificarse mediante una dirección URL. Dado que la página maestra y páginas de contenido podrían no existir en la misma carpeta, es importante que consista en abstenerse de utilizar direcciones URL relativas. Aunque es posible utilizar direcciones URL absolutas codificadas, realizar tan estrechamente se acopla con la dirección URL absoluta a la aplicación web. Si cambia la dirección URL absoluta - como ocurre con frecuencia cuando se mueva o implementar una aplicación web - tendrá que recordar volver atrás y actualizar las direcciones URL absolutas.

El enfoque ideal consiste en usar la tilde (`~`) para indicar la raíz de la aplicación. Asignan controles Web de ASP.NET que contienen propiedades relacionadas con la dirección URL de la `~` a la raíz de la aplicación en tiempo de ejecución. Internamente, los controles Web utilicen la `Control` la clase `ResolveClientUrl` método para generar una dirección URL relativa válida. Este método es público y está disponible en cada control de servidor (incluida la `Page` clase), por lo que se puede usar mediante programación de las clases de código subyacente, si es necesario.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Páginas maestras en ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Modificación de la dirección URL base en una página maestra](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Usar `ResolveClientUrl` en el marcado](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Anterior](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
[Siguiente](control-id-naming-in-content-pages-vb.md)
