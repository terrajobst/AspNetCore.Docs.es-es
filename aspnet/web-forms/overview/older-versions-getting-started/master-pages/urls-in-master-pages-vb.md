---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: Las direcciones URL en páginas maestras (VB) | Microsoft Docs
author: rick-anderson
description: Aborda cómo las direcciones URL en la página maestra pueden interrumpirse debido a la que están en un directorio relativo diferente que la página de contenido del archivo de página maestra. Examina el reajuste...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9094c6c2b1700f22fe29d8b341444e1178c9015f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395135"
---
<a name="urls-in-master-pages-vb"></a>Direcciones URL en páginas maestras (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) o [descargar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Aborda cómo las direcciones URL en la página maestra pueden interrumpirse debido a la que están en un directorio relativo diferente que la página de contenido del archivo de página maestra. Examina el reajuste de las direcciones URL a través de ~ en la sintaxis declarativa y con ResolveUrl y ResolveClientUrl mediante programación. (También ver


## <a name="introduction"></a>Introducción

En todos los ejemplos, hemos visto hasta ahora, que las páginas maestras y de contenido se han encontrado en la misma carpeta (carpeta de raíz del sitio Web). Pero no hay ningún motivo por qué las páginas maestras y de contenido deben estar en la misma carpeta. Sin duda puede crear páginas de contenido de las subcarpetas. De forma similar, puede crear un `~/MasterPages/` carpeta en la que colocar páginas maestras del sitio.

Un posible problema con la colocación de las páginas maestras y de contenido en diferentes carpetas implica rota las direcciones URL. Si la página maestra contiene las direcciones URL relativas en los hipervínculos, imágenes u otros elementos, el vínculo serán válido para las páginas de contenido que residen en una carpeta diferente. En este tutorial se examina el origen de este problema, así como soluciones alternativas.

## <a name="the-problem-with-relative-urls"></a>El problema con las direcciones URL relativas

Una dirección URL en una página web se dice que un *dirección URL relativa* si la ubicación del recurso apunta a es relativa a la ubicación de la página web en la estructura de carpetas del sitio Web. Cualquier dirección URL que no se inicia con una barra diagonal inicial (`/`) o un protocolo (como `http://`) es relativa, porque se ha resuelto el explorador basándose en la ubicación de la página web que contiene la dirección URL.

Por ejemplo, nuestro sitio Web tiene un `~/Images/` carpeta con un único archivo de imagen, `PoweredByASPNET.gif`. El archivo de página maestra `Site.master` tiene un `<img>` elemento en el `footerContent` región con el marcado siguiente:


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

El `src` atributo valor en el `<img>` elemento es una dirección URL relativa porque no se inicia con `/` o `http://`. En resumen, el `src` valor del atributo indica al explorador que busque en el `Images` subcarpeta para un archivo denominado `PoweredByASPNET.gif`.

Cuando se visita una página de contenido, el marcado anterior se envía directamente al explorador. Dedique unos minutos a visitar `About.aspx` y ver el código fuente HTML enviado al explorador. Encontrará que el mismo marcado exacto en la página maestra se ha enviado al explorador.


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

Si la página de contenido se encuentra en la carpeta raíz (como es `About.aspx`) todo funciona según lo esperado porque hay un `Images` subcarpeta relativa a la carpeta raíz. Sin embargo, las cosas desglosar de si la página de contenido se encuentra en una carpeta diferente a la página maestra. Para ilustrar esto, cree una subcarpeta denominada `Admin`. A continuación, agregue una página de contenido denominada `Default.aspx` a la `Admin` carpeta, asegurándose de enlazar la nueva página a la `Site.master` página maestra.

> [!NOTE]
> En el [ *especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial hemos creado una clase de página base personalizada denominada `BasePage` que establece automáticamente el título de la página de contenido (si lo no asignó explícitamente). No se olvide de tener clase de código subyacente de la página recién creada se derivan de `BasePage` para que pueda utilizar esta funcionalidad.


Después de haber creado esta página de contenido, el Explorador de soluciones debe ser similar a la figura 1.


![Se han agregado al proyecto una nueva carpeta y la página de ASP.NET](urls-in-master-pages-vb/_static/image1.png)

**Figura 01**: se han agregado al proyecto una nueva carpeta y la página de ASP.NET


A continuación, actualice el `Web.sitemap` que incluya un nuevo archivo `<siteMapNode>` entrada en esta lección. El siguiente XML muestra completo `Web.sitemap` marcado, que ahora incluye la adición de una tercera `<siteMapNode>` elemento.


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

Recién creado `Default.aspx` página debería tener cuatro controles de contenido correspondiente a las cuatro ContentPlaceHolders en `Site.master`. Agregue texto para el control de contenido que hacen referencia a la `MainContent` ContentPlaceHolder y, a continuación, visite la página a través de un explorador. Como se muestra en la figura 2, el explorador no se encuentra el `PoweredByASPNET.gif` archivo de imagen. ¿Qué está ocurriendo?

El `~/Admin/Default.aspx` página de contenido se envía el mismo HTML para el `footerContent` región ya era la `About.aspx` página:


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Dado que el `<img>` del elemento `src` el atributo es una dirección URL relativa, el explorador intenta buscar un `Images` carpeta relativa a la ubicación de la carpeta de la página web. En otras palabras, el explorador está buscando el archivo de imagen `Admin/Images/PoweredByASPNET.gif`.


[![No se encuentra el archivo de imagen PoweredByASPNET.gif](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Figura 02**: el `PoweredByASPNET.gif` imagen no se encuentra el archivo ([haga clic aquí para ver imagen en tamaño completo](urls-in-master-pages-vb/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Reemplazando las direcciones URL relativas con direcciones URL absolutas

Es el opuesto de una dirección URL relativa un *dirección URL absoluta*, que es el que comienza con una barra diagonal (`/`) o un protocolo como `http://`. Puesto que una dirección URL absoluta especifica la ubicación de un recurso desde un punto fijo conocido, la misma dirección URL absoluta es válida en cualquier página web, independientemente de la ubicación de la página web en la estructura de carpetas del sitio Web.

Para solucionar la imagen rota, se muestra en la figura 2, es necesario actualizar el `<img>` del elemento `src` atributo para que use una dirección URL absoluta en lugar de una relativa. Para determinar la dirección URL absoluta correcta, visite una de las páginas web en su sitio Web y examine la barra de direcciones. Como se muestra en la barra de direcciones en la figura 2, la ruta de acceso completa a la aplicación web es `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. Por lo tanto, es posible actualizar el `<img>` del elemento `src` atributo a cualquiera de las siguientes dos URL absolutas:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

Dedique unos minutos a actualizar el `<img>` del elemento `src` atributo a una dirección URL absoluta mediante uno de los formularios mostrados anteriormente y, a continuación, visite la `~/Admin/Default.aspx` página a través de un explorador. Esta vez el explorador correctamente buscar y mostrar el `PoweredByASPNET.gif` archivo de imagen (consulte la figura 3).


[![La imagen PoweredByASPNET.gif es ahora muestran](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Figura 03**: el `PoweredByASPNET.gif` imagen es ahora muestran ([haga clic aquí para ver imagen en tamaño completo](urls-in-master-pages-vb/_static/image7.png))


Si bien codificar de forma rígida en la dirección URL absoluta funciona, acople estrechamente el código HTML al servidor del sitio Web y la ubicación de carpeta, que puede cambiar. Mediante una dirección URL absoluta del formulario `http://localhost:3908/...` es frágil porque el número de puerto anterior localhost se selecciona automáticamente cada vez que se ha iniciado servidor para Web de desarrollo integrado ASP.NET de Visual Studio. De forma similar, la `http://localhost` parte solo es válida cuando se prueba localmente. Una vez que el código se implementa en un servidor de producción, la dirección URL base cambia a otra cosa, como `http://www.yourserver.com`. La dirección URL absoluta en el formulario `/ASPNET_MasterPages_Tutorial_04_VB/...` también padece la fragilidad misma porque a menudo, esta ruta de acceso de la aplicación es diferente entre los servidores de desarrollo y producción.

La buena noticia es que ofrece un método para generar una dirección URL relativa válida en tiempo de ejecución ASP.NET.

## <a name="usingandresolveclienturl"></a>Uso de`~`y`ResolveClientUrl`

En lugar de codificar una dirección URL absoluta, ASP.NET permite que los desarrolladores de páginas usar la tilde (`~`) para indicar la raíz de la aplicación web. Por ejemplo, anteriormente en este tutorial usa la notación `~/Admin/Default.aspx` en el texto para hacer referencia a la `Default.aspx` página en el `Admin` carpeta. El `~` indica que el `Admin` carpeta es una subcarpeta de raíz la aplicación web.

El `Control` la clase [ `ResolveClientUrl` método](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) toma una dirección URL y lo modifica para una dirección URL relativa adecuada para la página web en el que reside el control. Por ejemplo, al llamar a `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` desde `About.aspx` devuelve `Images/PoweredByASPNET.gif`. Llamarlo desde `~/Admin/Default.aspx`, sin embargo, se devuelve `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Dado que todos los controles de servidor ASP.NET que se derivan de la `Control` (clase), todos los controles de servidor tienen acceso a la `ResolveClientUrl` método. Incluso la `Page` clase se deriva de la `Control` (clase), lo que significa que puede usar este método directamente desde las clases de código subyacente de las páginas de ASP.NET.


### <a name="usingin-the-declarative-markup"></a>Uso de`~`en el marcado declarativo

Varios controles Web de ASP.NET incluyen propiedades relacionadas con la dirección URL: el control de hipervínculo tiene un `NavigateUrl` propiedad; la imagen de control tiene un `ImageUrl` propiedad; y así sucesivamente. Cuando se representan, estos controles pasan sus valores de propiedad relacionados con la dirección URL `ResolveClientUrl`. Por lo tanto, si estas propiedades contienen un `~` para indicar la raíz de la aplicación web, se modificará la dirección URL a una dirección URL relativa válida.

Tenga en cuenta que solo los controles de servidor ASP.NET transforman el `~` en sus propiedades relacionadas con la dirección URL. Si un `~` aparece en el marcado HTML estático, tales como `<img src="~/Images/PoweredByASPNET.gif" />`, el motor ASP.NET envía el `~` al explorador junto con el resto del contenido HTML. El explorador se da por supuesto que el `~` forma parte de la dirección URL. Por ejemplo, si el explorador recibe el marcado `<img src="~/Images/PoweredByASPNET.gif" />` se supone que hay una subcarpeta denominada `~` con una subcarpeta `Images` que contiene el archivo de imagen `PoweredByASPNET.gif`.

Para corregir el marcado de la imagen `Site.master`, sustituya la `<img>` elemento con un control Web de la imagen de ASP.NET. Establecer el control de imagen Web `ID` a `PoweredByImage`, sus `ImageUrl` propiedad `~/Images/PoweredByASPNET.gif`y su `AlternateText` propiedad a "Con tecnología ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Después de realizar este cambio en la página maestra, volver a visitar la `~/Admin/Default.aspx` página nuevo. Esta vez el `PoweredByASPNET.gif` archivo de imagen aparece en la página (consulte la figura 3). Cuando la Web de imagen de control es representa lo usa el `ResolveClientUrl` método para resolver su `ImageUrl` valor de propiedad. En `~/Admin/Default.aspx` el `ImageUrl` se convierte en una dirección URL relativa correspondiente, como el siguiente fragmento de código fuente HTML muestra:


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Además de utilizarse en las propiedades del control Web basado en URL, el `~` también se puede usar cuando se llama a la `Response.Redirect` y `Server.MapPath` métodos, entre otros. Además, el `ResolveClientUrl` se puede invocar el método directamente desde ASP.NET o marcado declarativo de la página principal, si es necesario; consulte [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)de entrada de blog [Using `ResolveClientUrl` en marcado](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Corrección de la página maestra restante de las direcciones URL relativas

Además el `<img>` elemento en el `footerContent` que se ha corregido, la página maestra contiene una dirección URL relativa más que requiere la atención. El `topContent` región incluye el vínculo "Master páginas tutoriales", que señala al `Default.aspx`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Dado que esta dirección URL es relativa, enviará al usuario la `Default.aspx` página en la carpeta de la página de contenido que están visitando. Para que este vínculo siempre señalan a `Default.aspx` en la carpeta raíz es necesario reemplazar el `<a>` controlar el elemento con un sitio Web de hipervínculo para que podemos usar el `~` notación.

Quitar el `<a>` elementos de marcado y agregue un control de hipervínculo en su lugar. Establecer el hipervínculo `ID` a `lnkHome`, sus `NavigateUrl` propiedad `~/Default.aspx`y su `Text` propiedad a "Tutoriales de páginas maestra".


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

Ya está. En este momento, todas las direcciones URL en la página maestra se basan correctamente cuando se representa con una página de contenido, independientemente de qué carpetas de la página maestra y la página de contenido se encuentran en.

### <a name="automatic-url-resolution-in-theheadsection"></a>Resolución automática de dirección URL en la`<head>`sección

En el [ *crear un diseño de todo el sitio usar páginas maestra* ](creating-a-site-wide-layout-using-master-pages-vb.md) tutorial se ha agregado un `<link>` a la `Styles.css` de archivos en el `<head>` región:


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Mientras el `<link>` del elemento `href` el atributo es relativo, se convierte automáticamente en una ruta de acceso adecuada en tiempo de ejecución. Como se explicó en la [ *especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial, el `<head>` región es en realidad un control del lado servidor, lo que le permite modificar el contenido de sus controles internos cuando se representa.

Para comprobarlo, volver a visitar la `~/Admin/Default.aspx` página y ver el código fuente HTML enviado al explorador. Como se muestra en el siguiente fragmento, el `<link>` del elemento `href` atributo se ha modificado automáticamente en una dirección URL relativa correspondiente `../Styles.css`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Resumen

Páginas maestras muy a menudo incluyen vínculos, imágenes y otros recursos externos que se deben especificar mediante una dirección URL. Dado que la página maestra y las páginas de contenido pueden no existir en la misma carpeta, es importante abstenerse de usar las direcciones URL relativas. Aunque es posible usar rígida de las direcciones URL absolutas, realizando fuertemente acopla la dirección URL absoluta a la aplicación web. Si cambia la dirección URL absoluta: igual que con frecuencia al mover o implementar una aplicación web: tendrá que acordarse de volver atrás y actualizar las direcciones URL absolutas.

El enfoque ideal es usar la tilde (`~`) para indicar la raíz de la aplicación. Controles Web de ASP.NET que contienen propiedades relacionadas con la dirección URL se asignan los `~` a la raíz de la aplicación en tiempo de ejecución. Internamente, los controles Web utilicen la `Control` la clase `ResolveClientUrl` método para generar una dirección URL relativa válida. Este método es público y está disponible en cada control de servidor (incluido el `Page` clase), por lo que se puede utilizar mediante programación desde las clases de código subyacente, si es necesario.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Páginas maestras en ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Modificación de la dirección URL base en una página principal](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Uso de `ResolveClientUrl` en el marcado](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [ *Sams Teach usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
> [Siguiente](control-id-naming-in-content-pages-vb.md)
