---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Volver a usar la interfaz de usuario mediante páginas maestras y parciales | Microsoft Docs
author: microsoft
description: Paso 7 examina maneras que podemos aplicar el principio DRY dentro de nuestras plantillas de vista para eliminar la duplicación de código, con plantillas de vista parcial y páginas maestras.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: f96d3f3059a442f977d4422797c872b865a55695
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391436"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Volver a usar la interfaz de usuario mediante páginas maestras y parciales
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 7 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 7 examina maneras que podemos aplicar el "principio DRY" dentro de nuestras plantillas de vista para eliminar la duplicación de código, con plantillas de vista parcial y páginas maestras.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>Paso 7 de NerdDinner: Parciales y páginas maestras

Una de las filosofías de diseño que abarca de ASP.NET MVC es el principio "Hacer no repitas" (conocido comúnmente como "DRY"). Un diseño seco ayuda a eliminar la duplicación de código y lógica, lo que en última instancia, hace que las aplicaciones más rápidas para crear y fáciles de mantener.

Ya hemos visto el principio DRY aplicado en algunos de nuestros escenarios de NerdDinner. Algunos ejemplos: nuestra lógica de validación se implementa dentro de nuestro nivel de modelo, lo que permite que se aplican en ambos editar y crear escenarios en nuestro controlador; volver a estamos usando la plantilla de vista de "NotFound" a través de los métodos de acción de edición, detalles y eliminación; estamos usando un patrón de convención de nomenclatura con nuestras plantillas de vista, lo que elimina la necesidad de especificar explícitamente el nombre cuando llamamos el método auxiliar View(); y se vuelve a utilizar la clase DinnerFormViewModel para ambos editar y creación escenarios de acción.

Ahora veamos maneras que podemos aplicar el "principio DRY" dentro de nuestras plantillas de vista para eliminar la duplicación de código allí también.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Volver a visitar nuestro editar y crear plantillas de vista

Actualmente estamos usando dos plantillas de vista diferentes: "Edit.aspx" y "Create.aspx –" para mostrar el formulario de la cena la interfaz de usuario. Una comparación visual rápida de ellos resalta similitud son. A continuación es el aspecto que tiene el formulario de creación:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Y aquí es el aspecto de nuestro formulario "Editar":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

¿No hay mucho de una diferencia está allí? Que no sea el texto de título y el encabezado, los controles de entrada y el diseño del formulario son idénticos.

Si se abrirán el "Edit.aspx" y "Create.aspx" plantillas de vista que se dará cuenta de que contienen código de control de diseño y la entrada de forma idéntica. Esta duplicación significa que se acabará por tener que realizar cambios dos veces cada vez que se presentan o cambiar una propiedad cena nueva - que no es válida.

### <a name="using-partial-view-templates"></a>Uso de plantillas de vista parcial

ASP.NET MVC admite la capacidad para definir plantillas de "vista parcial" que se pueden usar para encapsular la lógica de representación de la vista de una parte de una página secundaria. "Parciales" proporcionan una manera útil para definir la lógica de procesamiento de la vista una vez y, a continuación, volver a usarlo en varios lugares a través de una aplicación.

Para ayudar a "DRY-up" nuestro Edit.aspx y duplicación de plantilla de vista Create.aspx, podemos crear una plantilla de vista parcial denominada "DinnerForm.ascx" que encapsula el diseño del formulario y comunes a los elementos de entrada. Haremos esto con el botón secundario en nuestro directorio / Views/Dinners y eligiendo la "Add -&gt;vista" comando de menú:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Se mostrará el cuadro de diálogo "Agregar vista". Llamaremos a la nueva vista que desea crear "DinnerForm", active la casilla "Crear una vista parcial" en el cuadro de diálogo e indique que se le pasa una clase DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio creará una nueva plantilla de vista de "DinnerForm.ascx" para nosotros dentro del directorio "\Views\Dinners".

Se puede, a continuación, copiar y pegar el diseño del formulario duplicados o introducir el código de control de nuestras plantillas de vista Edit.aspx/ Create.aspx en nuestra nueva plantilla de vista parcial "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

A continuación, podemos actualizar nuestro editar y crear plantillas de vista para llamar a la plantilla parcial DinnerForm y eliminar la duplicación de formulario. Esto se logra mediante la llamada Html.RenderPartial("DinnerForm") dentro de nuestras plantillas de vista:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Puede calificar explícitamente la ruta de acceso de la plantilla parcial que desea al llamar a Html.RenderPartial (por ejemplo: ~ Views/Dinners/DinnerForm.ascx "). En nuestro código anterior, no obstante, estamos aprovechando el patrón de nomenclatura basado en convenciones en ASP.NET MVC y simplemente especificar "DinnerForm" como el nombre de la parcial para representar. Al hacer esto ASP.NET MVC buscará primero en el directorio basado en convenciones vistas (DinnersController sería/Views/Dinners). Si no encuentra la plantilla parcial allí, a continuación, será para él en el directorio /Views/Shared.

Cuando se llama Html.RenderPartial() con solo el nombre de la vista parcial, ASP.NET MVC pasará a la vista parcial los mismos modelos y ViewData diccionario objetos utilizados por la plantilla de vista que realiza la llamada. Como alternativa, existen versiones sobrecargadas de Html.RenderPartial() que le permiten pasar un objeto de modelo alternativo o de diccionario ViewData de la vista parcial a usar. Esto es útil para escenarios donde solo se desea pasar un subconjunto del modelo completo/ViewModel.

| **Tema de lado: ¿Por qué &lt;%%&gt; en lugar de &lt;% = %&gt;?** |
| --- |
| Uno de los aspectos sutiles que es posible que haya observado con el código anterior es que estamos usando un &lt;%%&gt; bloquear en lugar de un &lt;% = %&gt; bloquear al llamar a Html.RenderPartial(). &lt;% = %&gt; bloques en ASP.NET indican que un programador desea representar un valor especificado (por ejemplo: &lt;% = "Hello" %&gt; representaría "Hello"). &lt;%%&gt; bloques en su lugar, indican que el desarrollador desea ejecutar el código, y cualquier elemento representado salida dentro de ellos debe realizarse explícitamente (por ejemplo: &lt;Response.Write("Hello") %&gt;. La razón por la que usamos un &lt;%%&gt; es bloque con nuestro código Html.RenderPartial anterior porque el método Html.RenderPartial() no devuelve una cadena y, en su lugar genera el contenido directamente a la plantilla de vista que realiza la llamada del flujo de salida. Esto lo hace por motivos de eficiencia de rendimiento y, por lo que evita la necesidad de crear un objeto de cadena temporal (potencialmente muy grandes). Esto reduce el uso de memoria y mejora el rendimiento general de la aplicación. Un error común al usar Html.RenderPartial() consiste en olvide agregar un punto y coma al final de la llamada cuando está dentro de un &lt;%%&gt; bloque. Por ejemplo, este código producirá un error del compilador: &lt;Html.RenderPartial("DinnerForm") %&gt; en su lugar, deberá escribir: &lt;% Html.RenderPartial("DinnerForm"); %&gt; esto es porque &lt;%%&gt; bloques son instrucciones de código independiente y cuando se usa código C# instrucciones deben terminarse con punto y coma. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Uso de plantillas de vista parcial para aclarar el código

Hemos creado la plantilla de vista parcial "DinnerForm" para evitar la duplicación de lógica de procesamiento de la vista en varios lugares. Este es el motivo más común para crear plantillas de vista parcial.

A veces, todavía tiene sentido crear las vistas parciales, incluso cuando solo se llama en un solo lugar. A menudo muy complicado ver plantillas pueden convertirse en mucho más fáciles de leer cuando su lógica de procesamiento de la vista se extraen y dividir en uno o más bien denominado parcial de plantillas.

Por ejemplo, considere el siguiente fragmento de código desde el archivo Site.master en nuestro proyecto (lo que estamos buscando en breve). El código es relativamente sencillo leer – en parte porque vincular la lógica para mostrar un inicio de sesión o cierre de sesión en la parte superior derecha de la pantalla se encapsula dentro de la parte "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Cada vez que encuentre confundirse intentando entender el marcado html/código dentro de una plantilla de vista, considere si no sería más claro si algunos de los se ha extraído y refactorizado en las vistas parciales con un nombre adecuado.

### <a name="master-pages"></a>Páginas maestras

Además de admitir las vistas parciales, ASP.NET MVC también admite la capacidad para crear plantillas de "página principal" que pueden utilizarse para definir el diseño común y el código html de nivel superior de un sitio. Marcador de posición, a continuación, se pueden agregar controles a la página maestra para identificar regiones reemplazables que se pueden invalidadas o "rellena" las vistas de contenido. Esto proporciona una manera muy eficaz (y SECA) para aplicar un diseño común en una aplicación.

De forma predeterminada, nuevos proyectos de MVC de ASP.NET tienen una plantilla de página maestra que se agregan automáticamente a ellos. Esta página principal se denomina "Site.master" y la vida dentro de la carpeta \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

El archivo Site.master predeterminado como el siguiente. Define el código html exterior del sitio, junto con un menú de navegación en la parte superior. Contiene dos controles que puede reemplazar el marcador de posición de contenido: uno para el título y la otra para la que se debe reemplazar el contenido de una página principal:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Todas las plantillas de vista que hemos creado para nuestra aplicación NerdDinner ("List", "Detalles", "Editar", "Crear", "NotFound", etcetera) se basaban en esta plantilla Site.master. Esto se indica mediante el atributo "MasterPageFile" que se agregó de forma predeterminada a la parte superior &lt;% @ Page %&gt; directiva cuando creamos nuestras vistas mediante el cuadro de diálogo "Agregar vista":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Esto significa que podemos cambiar el contenido de Site.master y tienen los cambios automáticamente se aplica y usar cuando se procesa alguna de nuestras plantillas de vista.

Vamos a actualizar sección de encabezado de nuestra Site.master para que el encabezado de nuestra aplicación es "NerdDinner" en lugar de "Mi aplicación de MVC". Vamos a actualizar también el menú de navegación para que la primera pestaña es "Buscar una cena" (controladas por el método de acción de HomeController Index()) y vamos a agregar una nueva pestaña denominada "Host una cena" (controladas por el método de acción de DinnersController Create()):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Cuando se guarda el archivo Site.master y actualizar nuestro explorador veremos el encabezado de cambios se reflejen en todas las vistas dentro de nuestra aplicación. Por ejemplo:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Y con el */dinners/Edit / [id]* dirección URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Paso siguiente

Parciales y páginas maestras proporcionan opciones muy flexibles que permiten organizar limpiamente vistas. Encontrará que ayudan a evitar la duplicación de vista de contenido / de código y que sea más fácil leer y mantener las plantillas de vista.

Ahora vamos a volver a visitar el escenario de lista que se generó anteriormente y habilitar la compatibilidad con la paginación escalable.

> [!div class="step-by-step"]
> [Anterior](use-viewdata-and-implement-viewmodel-classes.md)
> [Siguiente](implement-efficient-data-paging.md)
