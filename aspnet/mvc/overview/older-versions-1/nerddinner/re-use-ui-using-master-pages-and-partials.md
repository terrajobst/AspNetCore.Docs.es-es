---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Volver a usar la interfaz de usuario mediante páginas maestras y parciales | Documentos de Microsoft
author: microsoft
description: Paso 7 examina maneras que podemos aplicar el principio SECA en nuestras plantillas de vista para eliminar la duplicación de código, uso de plantillas de vista parcial y páginas maestras.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: ade655f3a4a429360b678d02fb564ac9cf255d42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870016"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Volver a usar la interfaz de usuario mediante páginas maestras y parciales
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 7 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 7 examina maneras que podemos aplicar el "principio SECA" dentro de nuestras plantillas de vista para eliminar la duplicación de código, uso de plantillas de vista parcial y páginas maestras.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner paso 7: Parciales y las páginas maestras

Una de las filosofías de diseño que acoja de ASP.NET MVC es el principio "Hacer no repitas" (que normalmente se denomina "SECA"). Un diseño seco ayuda a eliminar la duplicación de código y la lógica, que en última instancia hace que las aplicaciones más rápidas para crear y fáciles de mantener.

Ya hemos visto el principio seco aplicado en algunos de los escenarios de NerdDinner. Algunos ejemplos: nuestra lógica de validación se implementa en el nivel de modelo, lo que permite que se aplican en ambos editar y crear escenarios en nuestro controlador; volver a estamos utilizando la plantilla de vista de "NotFound" a través de los métodos de acción de edición, detalles y Delete; Estamos utilizando un modelo de convención de nomenclatura con nuestras plantillas de vista, lo que elimina la necesidad de especificar explícitamente el nombre cuando se llama al método de aplicación auxiliar View(); y se vuelve a utilizar la clase DinnerFormViewModel para ambos editar y crear escenarios de acción.

Ahora veamos maneras que podemos aplicar el "principio SECA" dentro de nuestras plantillas de vista para eliminar la duplicación del código también.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Volver a visitar nuestro editar y crear plantillas de vista

Actualmente estamos usando dos plantillas de vista diferentes: "Edit.aspx" y "Create.aspx:" para mostrar el formulario de cena interfaz de usuario. Una comparación visual rápida de ellos resalta la similitud son. A continuación se muestra el aspecto de la forma de crear:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Y aquí es el aspecto de nuestro formulario "Editar":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

¿No hay mucho de una diferencia está allí? Que no sea el texto de título y el encabezado, los controles de diseño y la entrada de formulario son idénticos.

Si se abrirán la "Edit.aspx" y "Create.aspx" Ver plantillas que buscaremos que contienen código de control de diseño y la entrada de forma idéntica. Esta duplicación significa que se acaban por tener que realizar cambios dos veces cada vez que se introducen o cambia una nueva propiedad cena - que no es adecuada.

### <a name="using-partial-view-templates"></a>Uso de plantillas de vista parcial

ASP.NET MVC admite la capacidad para definir plantillas de "vista parcial" que pueden utilizarse para encapsular la lógica de representación de vista de una parte de una página secundaria. "Parciales" proporcionan una forma útil definir la lógica de procesamiento de la vista una vez y, a continuación, volver a usarlo en varios lugares en toda la aplicación.

Para ayudar a "SECA-up" nuestro Edit.aspx y duplicación de plantillas de vista de Create.aspx, podemos crear una plantilla de vista parcial con el nombre "DinnerForm.ascx" que encapsula el diseño del formulario y los datos comunes a los elementos de entrada. Deberá hacerlo, haga doble clic en el directorio de cenas/vistas/y elija el "Add -&gt;vista" comando de menú:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Se mostrará el cuadro de diálogo "Agregar vista". Llamaremos a la nueva vista que desea crear "DinnerForm", active la casilla "Crear una vista parcial" en el cuadro de diálogo e indique que se le pase una clase DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Cuando se hace clic en el botón "Agregar", Visual Studio creará una nueva plantilla de vista de "DinnerForm.ascx" para que podamos dentro del directorio "\Views\Dinners".

Se puede, a continuación, copiar y pegar el diseño del formulario duplicados / código de control de nuestras plantillas de vista de Edit.aspx/ Create.aspx de entrada en la nueva plantilla de la vista parcial "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

A continuación, podemos actualizar nuestras plantillas de vista de editar y crear para llamar a la plantilla parcial DinnerForm y eliminar la duplicación de formulario. Podemos hacer esto por Html.RenderPartial("DinnerForm") que realiza la llamada dentro de nuestras plantillas de vista:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Puede calificar explícitamente la ruta de acceso de la plantilla parcial que desee al llamar a Html.RenderPartial (por ejemplo: ~ Views/Dinners/DinnerForm.ascx "). En el código anterior, sin embargo, estamos aprovechando el patrón de nomenclatura basada en convenciones dentro de ASP.NET MVC y simplemente especificar "DinnerForm" como el nombre de parte para representar. Al hacer esto ASP.NET MVC busca primero en el directorio vistas basada en convenciones (DinnersController sería/vistas/cenas). Si no encuentra la plantilla parcial no existe, a continuación, mirará para él en el directorio /Views/Shared.

Cuando se llama a Html.RenderPartial() solamente el nombre de la vista parcial, ASP.NET MVC pasará a la vista parcial el mismo modelo y ViewData diccionario los objetos utilizados por la plantilla de vista que realiza la llamada. Como alternativa, hay versiones sobrecargadas de Html.RenderPartial() que permiten pasar un objeto de modelo alternativo o el diccionario ViewData de la vista parcial para utilizarla. Esto es útil en escenarios donde solo se desea pasar un subconjunto de modelo/ViewModel completa.

| **Tema de lado: ¿Por qué &lt;%%&gt; en lugar de &lt;% = %&gt;?** |
| --- |
| Una de las cosas sutiles que es posible que haya observado con el código anterior es que estamos usando un &lt;%%&gt; bloquear en lugar de un &lt;% = %&gt; bloquear al llamar a Html.RenderPartial(). &lt;% = %&gt; bloques en ASP.NET indican que un programador desea representar un valor especificado (por ejemplo: &lt;% = "Hello" %&gt; podría invalidar "Hello"). &lt;%%&gt; bloques en su lugar, indican que el desarrollador desea ejecutar el código, y cualquier elemento representado salida dentro de ellos debe realizarse explícitamente (por ejemplo: &lt;Response.Write("Hello") %&gt;. La razón, usamos un &lt;%%&gt; es bloque con nuestro código Html.RenderPartial anterior porque el método Html.RenderPartial() no devuelve una cadena y genera el contenido directamente a la plantilla de vista que realiza la llamada del flujo de salida. Esto lo hace por motivos de eficiencia de rendimiento y, por lo que evita la necesidad de crear un objeto de cadena temporal (pueden ser muy grandes). Esto reduce el uso de memoria y mejora el rendimiento general de la aplicación. Un error común al utilizar Html.RenderPartial() es olvidarse de agregar un punto y coma al final de la llamada cuando esté dentro de un &lt;%%&gt; bloque. Por ejemplo, este código provocará un error del compilador: &lt;Html.RenderPartial("DinnerForm") %&gt; en su lugar, tiene que escribir: &lt;% Html.RenderPartial("DinnerForm"); %&gt; esto es porque &lt;%%&gt; bloques son instrucciones de código independiente y cuando se usa código C#, necesitan las instrucciones que debe finalizar con un punto y coma. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Uso de plantillas de vista parcial para aclarar el código

Hemos creado la plantilla de vista parcial "DinnerForm" para evitar la duplicación de lógica de representación de vista en varios lugares. Esta es la razón más común para crear plantillas de vista parcial.

A veces, todavía tiene sentido crear vistas parciales incluso si sólo se les llame en un único lugar. Plantillas de vista muy complicada a menudo pueden ser mucho más fáciles de leer cuando su lógica de procesamiento de la vista se extrae y crea una partición en una o más bien con el nombre parcial de plantillas de.

Por ejemplo, considere el siguiente fragmento de código desde el archivo Site.master en nuestro proyecto (que nos encargaremos en poco tiempo). El código es relativamente sencilla para leer – en parte porque la lógica para mostrar un inicio de sesión o cierre de sesión de vínculo en la parte superior derecha de la pantalla se encapsula dentro de la parte "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Cada vez que encuentre confundirse al intentar comprender el marcado html/código dentro de una plantilla de vista, considere si sería más clara si algunos de los se ha extraído y refactorizado en las vistas parciales bien con nombre.

### <a name="master-pages"></a>Páginas maestras

Además de admitir las vistas parciales, ASP.NET MVC también admite la capacidad para crear plantillas de "página maestra" que pueden usarse para definir el diseño común y el código html de nivel superior de un sitio. Marcador de posición, a continuación, pueden agregarse a la página maestra para identificar regiones reemplazables que se pueden reemplazar o "rellena" en las vistas de controles de contenido. Esto proporciona una manera muy eficaz (y SECA) para aplicar un diseño común en una aplicación.

De forma predeterminada, nuevos proyectos de ASP.NET MVC tienen una plantilla de página maestra que se agregan automáticamente a ellas. Esta página principal se denomina "Site.master" y vida dentro de la carpeta \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

El archivo Site.master predeterminado es similar a continuación. Define el html exterior del sitio, junto con un menú de navegación en la parte superior. Contiene dos controles de marcador de posición de contenido reemplazables: uno para el título y la otra para el que se debe reemplazar el contenido de una página principal:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Todas las plantillas de vista que hemos creado para nuestra aplicación NerdDinner ("List", "Detalles", "Editar", "Crear", "NotFound", etcetera) se ha basado en esta plantilla Site.master. Esto se indica mediante el atributo "MasterPageFile" que se agregó de forma predeterminada a la parte superior &lt;% @ Page %&gt; directiva cuando creamos nuestras vistas mediante el cuadro de diálogo "Agregar vista":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Esto significa que podemos cambiar el contenido de Site.master y tiene los cambios automáticamente se aplica y usar cuando se procesen cualquiera de las plantillas de vista.

Vamos a actualizar sección de encabezado de nuestra Site.master para que el encabezado de nuestra aplicación es "NerdDinner" en lugar de "Mi aplicación de MVC". Vamos a actualizar el menú de navegación para que la primera pestaña es "Buscar una cena" (controlado por el método de acción del HomeController Index()) y vamos a agregar una ficha nueva denominada "Host unos cena" (controlado por método de acción del DinnersController Create()):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Cuando se guarda el archivo Site.master y actualizar nuestro navegador veremos nuestro encabezado cambios mostrar hasta en todas las vistas dentro de nuestra aplicación. Por ejemplo:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Y con el */Dinners/editar / [id]* dirección URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Paso siguiente

Parciales y las páginas maestras proporcionan opciones muy flexibles que permiten organizar limpiamente vistas. Encontrará que ayudan a evitar la duplicación de vista de contenido / código y que las plantillas de vista sea más fácil de leer y mantener.

Ahora vamos a volver a visitar el escenario de lista que se generó anteriormente y habilitar la compatibilidad con la paginación escalable.

> [!div class="step-by-step"]
> [Anterior](use-viewdata-and-implement-viewmodel-classes.md)
> [Siguiente](implement-efficient-data-paging.md)
