---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: Anidar páginas maestras (C#) | Microsoft Docs
author: rick-anderson
description: Muestra cómo anidar una página maestra dentro de otra.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a93f8a0dc829f45eaa08a356f01f51df5443e29
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379078"
---
<a name="nested-master-pages-c"></a>Páginas maestras anidadas (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip) o [descargar PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> Muestra cómo anidar una página maestra dentro de otra.


## <a name="introduction"></a>Introducción

En el transcurso de los últimos nueve tutoriales hemos visto cómo implementar un diseño de todo el sitio con páginas maestras. En pocas palabras, las páginas maestras nos permiten, el desarrollador de páginas, para definir el marcado comunes en la página maestra junto con las regiones específicas que puede personalizarse según el contenido de página por el contenido de página. Los controles ContentPlaceHolder en una página maestra indican las regiones personalizables; el formato personalizado para los controles ContentPlaceHolder definidos en la página de contenido a través de los controles de contenido.

Las técnicas de página maestra que hemos analizado hasta ahora son excelentes si tiene un diseño único que se usa en todo el sitio. Sin embargo, muchos sitios Web grandes tienen un diseño de sitio que se personaliza a través de distintas secciones. Por ejemplo, considere una aplicación sanitaria usada por el personal de hospital para administrar la facturación, actividades y la información del paciente. Puede haber tres tipos de páginas web en esta aplicación:

- Las páginas específicas de los miembros de personal donde los miembros del personal pueden actualizar la disponibilidad, ver programaciones o solicitar vacaciones.
- Páginas de paciente específicas donde los miembros del personal ver o edición la información de un paciente específico.
- Las páginas específicas de facturación donde contables revisión actual de notificación de Estados e informes financieros.

Todas las páginas pueden compartir un diseño común, como un menú en la parte superior y una serie de vínculos usados con frecuencia a lo largo de la parte inferior. Pero el personal, paciente y las páginas específicas de facturación que necesite personalizar este diseño genérico. Por ejemplo, quizás todas las páginas de personal específico deben incluir una lista de calendario y tareas que muestra la disponibilidad y la programación diaria del usuario ha iniciado sesión actualmente. Quizás todas las páginas específicas del paciente necesitan mostrar el nombre, la dirección y la información de seguros para el paciente cuya información se está editando.

Es posible crear tales diseños personalizados mediante el uso de *páginas maestras anidadas*. Para implementar el escenario anterior, ejecutaríamos mediante la creación de una página maestra que definen el contenido de diseño, el menú y el pie de página de todo el sitio, con ContentPlaceHolders definen las regiones personalizables. A continuación, se crearía tres páginas maestras anidadas, uno para cada tipo de página web. Cada página maestra anidada definiría el contenido entre el tipo de páginas de contenido que utilice la página principal. En otras palabras, la página maestra anidada específica del paciente para páginas de contenido incluye marcado y lógica de programación para mostrar información acerca de los pacientes que se está editando. Al crear una nueva página específica del paciente se enlazaría a esta página maestra anidada.

Este tutorial comienza resaltando las ventajas de las páginas maestras anidadas. A continuación, se muestra cómo crear y usar las páginas maestras anidadas.

> [!NOTE]
> Páginas maestras anidadas han sido posibles desde la versión 2.0 de .NET Framework. Sin embargo, Visual Studio 2005 no incluían compatibilidad en tiempo de diseño para las páginas maestras anidadas. La buena noticia es que Visual Studio 2008 ofrece una experiencia de tiempo de diseño para las páginas maestras anidadas. Si está interesado en usar páginas maestras anidadas pero todavía usa Visual Studio 2005, visite [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog, [sugerencias para las páginas maestras anidadas en tiempo de diseño de VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Las ventajas de las páginas maestras anidadas

Muchos sitios Web tienen un diseño general del sitio, así como más personalizados diseños específicos a ciertos tipos de páginas. Por ejemplo, en nuestra aplicación web de demostración hemos creado una sección de administración rudimentaria (las páginas en el `~/Admin` carpeta). Actualmente, las páginas web en el `~/Admin` carpeta utilizan la misma página maestra como esas páginas no está en la sección de administración (es decir, `Site.master` o `Alternate.master`, según la selección del usuario).

> [!NOTE]
> Por ahora, supongamos que nuestro sitio tiene una sola página maestra, `Site.master`. Abordaremos mediante páginas maestras anidadas con páginas maestras dos (o más) a partir de "Utilizando un anidada Master página para la sección de administración" más adelante en este tutorial.


Imagine que se nos pidió que personalizar el diseño de las páginas de administración para incluir información adicional o vínculos que podrían no estar presentes en otras páginas del sitio. Hay cuatro técnicas para implementar este requisito:

1. Agregue manualmente la información específica de la administración y vínculos a cada página de contenido en el `~/Admin` carpeta.
2. Actualización de la `Site.master` página maestra para incluir la información específica de la sección de administración y los vínculos y, a continuación, agregue código a la página maestra para mostrar u ocultar estas secciones en función de si una de las páginas de administración se visita.
3. Crear una nueva página maestra específicamente para la sección de administración, copie sobre el marcado de `Site.master`, agregue la información específica de la sección de administración y los vínculos y, a continuación, actualice las páginas de contenido en el `~/Admin` carpeta que desea utilizar este nuevo patrón página.
4. Crear una página maestra anidada que se enlaza a `Site.master` y tienen las páginas de contenido en el `~/Admin` anidadas de carpeta, usar esta nueva página maestra. Esta página maestra anidada podría incluir solo la información y vínculos adicionales específicas de las páginas de administración y no tendría que repetir el marcado ya definido en `Site.master`.

La primera opción es menos agradable ya. El objetivo de utilizar páginas principales es pasar de tener que copiar y pegar marcado comunes a las nuevas páginas ASP.NET manualmente. La segunda opción es aceptable, pero hace que la aplicación sea más difícil de mantener que rellena las páginas maestras con marcado que se muestra solo en algunas ocasiones y requiere que los desarrolladores editando la página maestra para evitar este marcado y tenga que recordar cuando, exactamente, cierta marcado aparece frente a cuando está oculto. Este enfoque sería menos tenable como personalizaciones desde cada vez más tipos de páginas web necesarios para cumplir poniendo en esta página maestra única.

La tercera opción quita el desorden y complejidad emite el apareció con la segunda opción. Sin embargo, la principal desventaja de la opción de tres es que será necesario copiar y pegar el diseño común de `Site.master` a la nueva página principal de específicos de la sección de administración. Si, más adelante decide cambiar el diseño de todo el sitio que se tienen que recordar que la cambie en dos lugares.

La cuarta opción, páginas maestras anidadas, nos ofrecen el mejor de las opciones de la segunda y terceros. La información de diseño de todo el sitio se mantiene en un archivo: la página principal de nivel superior: mientras el contenido específico de determinadas regiones se divide en distintos archivos.

Este tutorial comienza con una mirada a la creación y uso de una página maestra anidada simple. Creamos una nueva página maestra nivel superior, dos páginas maestras anidadas y dos páginas de contenido. A partir de "Utilizando un anidados Master página para la sección de administración", nos centramos en nuestra arquitectura de página maestra existente para incluir el uso de las páginas maestras anidadas de actualización. En concreto, se crea una página maestra anidada y usarlo para incluir contenido personalizado adicional para las páginas de contenido en el `~/Admin` carpeta.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Paso 1: Creación de una página principal de nivel superior sencilla

Creación de un patrón anidado basado en uno del patrón existente de páginas y, a continuación, actualizar una página de contenido existente para usar esta nueva página maestra anidada en lugar de la página principal de nivel superior implica cierta complejidad porque las páginas de contenido existentes ya esperan determinados Controles ContentPlaceHolder definidos en la página principal de nivel superior. Por lo tanto, la página maestra anidada también debe incluir los mismos controles ContentPlaceHolder con los mismos nombres. Además, nuestra aplicación de demostración determinado tiene dos páginas maestras (`Site.master` y `Alternate.master`) que se asignan dinámicamente a una página de contenido según las preferencias del usuario, lo que aumenta aún más esta complejidad. Examinaremos la actualización de la aplicación existente para utilizar las páginas maestras anidadas más adelante en este tutorial, pero vamos a centrarnos primero en una simple anidados ejemplo páginas maestras.

Cree una carpeta nueva denominada `NestedMasterPages` y, a continuación, agregue un nuevo archivo de página maestra a la carpeta denominada `Simple.master`. (Consulte la figura 1 para una captura de pantalla del explorador de soluciones después de han agregado esta carpeta y archivo). Arrastre el `AlternateStyles.css` archivo de hoja de estilo desde el Explorador de soluciones en el diseñador. Esto agrega un `<link>` elemento para el archivo de hoja de estilos en el `<head>` elemento tras el cual la página maestra `<head>` marcado del elemento debe ser similar:


[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

A continuación, agregue el siguiente marcado dentro del formulario Web de `Simple.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

Este marcado muestra un vínculo titulado "Nested Master Pages (Simple)" en la parte superior de la página en una fuente grande blanco sobre un fondo azul marino. Bajo el que el `MainContent` ContentPlaceHolder. La figura 1 muestra la `Simple.master` página principal cuando se carga en el Diseñador de Visual Studio.


[![La página maestra anidada define contenido determinado a las páginas en la sección de administración](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**Figura 01**: The anidados Master página define contenido específico a las páginas en la sección de administración ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Paso 2: Creación de una Simple página maestra anidada

`Simple.master` contiene dos controles ContentPlaceHolder: el `MainContent` ContentPlaceHolder agregamos dentro del formulario Web junto con la `head` ContentPlaceHolder en el `<head>` elemento. Si tuviéramos que crear una página de contenido y enlazarlo a `Simple.master` la página de contenido tendría dos controles de contenido que hacen referencia a los dos ContentPlaceHolders. De forma similar, si se crea una página maestra anidada y enlácelo al `Simple.master` , a continuación, la página maestra anidada tendrá dos controles de contenido.

Vamos a agregar una nueva página maestra anidada para el `NestedMasterPages` carpeta denominada `SimpleNested.master`. Haga doble clic en el `NestedMasterPages` carpeta y seleccione Agregar nuevo elemento. Se abrirá el cuadro de diálogo Agregar nuevo elemento que se muestra en la figura 2. Seleccione el tipo de plantilla de página maestra y escriba el nombre de la nueva página maestra. Para indicar que la nueva página principal debe ser una página maestra anidada, active la casilla de verificación "Seleccionar la página principal".

A continuación, haga clic en el botón Agregar. Esto mostrará el mismo, seleccione un cuadro de diálogo de página maestra que verá al enlazar una página de contenido a una página maestra (consulte la figura 3). Elija la `Simple.master` página maestra en el `NestedMasterPages` carpeta y haga clic en Aceptar.

> [!NOTE]
> Si ha creado su sitio Web ASP.NET mediante el modelo de proyecto de aplicación Web en lugar del modelo de proyecto de sitio Web no verá la casilla de verificación "Seleccionar la página principal" en el cuadro de diálogo Agregar nuevo elemento que se muestra en la figura 2. Para crear una página maestra anidada al utilizar el modelo de proyecto de aplicación Web debe elegir la plantilla de página maestra anidada (en lugar de la plantilla de página maestra). Después de seleccionar la plantilla anidada página maestra y hacer clic en Agregar, el mismo seleccionar una página maestra, aparecerá el cuadro de diálogo que se muestra en la figura 3.


[![Compruebe el &quot;seleccionar la página maestra&quot; casilla de verificación para agregar una página maestra anidada](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**Figura 02**: Active la casilla "Seleccionar la página principal" para agregar una página maestra anidada ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image6.png))


[![Enlazar la página maestra anidada a la página principal Simple.master](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**Figura 03**: enlazar a la página maestra anidada para el `Simple.master` página maestra ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image9.png))


Marcado declarativo de la página maestra anidada, que se muestra a continuación, contiene dos controles de contenido que hacen referencia a controles ContentPlaceHolder de dos del nivel superior de la página maestra.


[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

Excepto para el `<%@ Master %>` la directiva, marcado declarativo de la página maestra anidada inicial es idéntico al código de marcado que se generó inicialmente al enlazar una página de contenido a la misma página principal de nivel superior. Al igual que una página de contenido `<%@ Page %>` directiva, el `<%@ Master %>` directiva aquí incluye un `MasterPageFile` atributo que especifica la página principal de la página maestra anidada primaria. La principal diferencia entre la página maestra anidada y una página de contenido enlazados a la misma página de nivel superior principal es que la página maestra anidada puede incluir controles ContentPlaceHolder. Controles de la página maestra anidada ContentPlaceHolder definen las regiones donde las páginas de contenido pueden personalizar el marcado.

Actualice esta página maestra anidada para que se muestre el texto "Hello, from SimpleNested!" en el control de contenido que se corresponde con el `MainContent` controles ContentPlaceHolder.


[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

Después de realizar esta adición, guarde la página maestra anidada y, a continuación, agregue una nueva página de contenido para el `NestedMasterPages` carpeta denominada `Default.aspx`y enlazarlo a la `SimpleNested.master` página maestra. Al agregar esta página se sorprenderá al ver que no contiene los controles de contenido (consulte la figura 4)! Solo puede tener acceso una página de contenido su *primario* dominar ContentPlaceHolders de la página. `SimpleNested.master` no contiene todos los controles ContentPlaceHolder; por lo tanto, cualquier página de contenido enlazado a esta página maestra no puede contener los controles de contenido.


[![La nueva página de contenido no contiene ningún control de contenido](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**Figura 04**: el nuevo contenido página contiene sin controles de contenido ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image12.png))


Lo que debemos hacer es actualizar la página maestra anidada (`SimpleNested.master`) para incluir controles ContentPlaceHolder. Normalmente, querrá que las páginas maestras anidadas para incluir un ContentPlaceHolder para cada ContentPlaceHolder definido por su página principal del elemento primario, lo que permite que su página maestra secundaria o una página de contenido para trabajar con cualquiera de ContentPlaceHolder el nivel superior de la página maestra controles.

Actualización de la `SimpleNested.master` página maestra para incluir un ContentPlaceHolder en sus dos controles de contenido. Asigne a los controles ContentPlaceHolder el mismo nombre que el control ContentPlaceHolder a que su control de contenido hace referencia. Es decir, agregue un control ContentPlaceHolder denominado `MainContent` al contenido de control en `SimpleNested.master` que hace referencia a la `MainContent` ContentPlaceHolder en `Simple.master`. Lo mismo en el control de contenido que hace referencia a la `head` ContentPlaceHolder.

> [!NOTE]
> Aunque recomiendo la nomenclatura de los controles ContentPlaceHolder en la página maestra anidada igual que el ContentPlaceHolders en la página principal de nivel superior, la simetría de nomenclatura no es necesaria. Puedes usar los controles ContentPlaceHolder en la página maestra anidada cualquier nombre que desee. Sin embargo, le resulte más fácil recordar qué ContentPlaceHolders se corresponden con las regiones de la página si mi página principal de nivel superior y las páginas maestras anidadas utilizan los mismos nombres.


Después de realizar estas adiciones su `SimpleNested.master` marcado declarativo de la página principal debe ser similar al siguiente:


[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

Eliminar el `Default.aspx` página que acabamos de crear contenido y, a continuación, vuelva a agregarlo, enlazarlo a la `SimpleNested.master` página maestra. Esta vez Visual Studio agrega dos controles de contenido para el `Default.aspx`, ahora que hacen referencia a la ContentPlaceHolders definido en `SimpleNested.master` (consulte la figura 6). Agregue el texto, "Hello, from Default.aspx!" en el contenido del control que se hace referencia `MainContent`.

La figura 5 muestra las tres entidades involucradas en esto - `Simple.master`, `SimpleNested.master`, y `Default.aspx` - y cómo se relacionan entre sí. Como se muestra en el diagrama, la página maestra anidada implementa los controles de contenido para ContentPlaceHolder de su elemento primario. Si estas regiones deben poder acceder a la página de contenido, la página maestra anidada debe agregar su propio ContentPlaceHolders a los controles de contenido.


[![Las páginas maestras anidadas y de nivel superior dictan el diseño de la página de contenido](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**Figura 05**: las páginas maestras anidadas y de nivel superior dictan el diseño de la página de contenido ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image15.png))


Este comportamiento muestra cómo una página de contenido o página maestra solo está al corriente de su página principal del elemento primario. Este comportamiento también se indica mediante el Diseñador de Visual Studio. Figura 6 se muestra el diseñador para `Default.aspx`. Aunque el diseñador muestra claramente qué regiones son editables de la página de contenido y lo que no son partes, no eliminar la ambigüedad ¿cuáles son las regiones que no son editables de la página maestra anidada y cuáles son las regiones de la página principal de nivel superior.


[![Contenido página ahora incluye controles de contenido para ContentPlaceHolders la página maestra anidada](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**Figura 06**: el contenido página ahora incluye controles de contenido para ContentPlaceHolders el anidado de la página maestra ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Paso 3: Agregar una segunda Simple página maestra anidada

La ventaja de las páginas maestras anidadas es más evidente cuando hay varias páginas maestras anidadas. Para ilustrar esta ventaja, cree otra página maestra anidada en la `NestedMasterPages` carpeta; la página principal de esta nueva de nombre anidado `SimpleNestedAlternate.master` y enlazarlo a la `Simple.master` página maestra. Agregar controles ContentPlaceHolder en dos controles de contenido de la página maestra anidada como hicimos en el paso 2. Agregue también el texto, "Hello, from SimpleNestedAlternate!" en el control de contenido que se corresponde con el nivel superior de la página maestra `MainContent` ContentPlaceHolder. Después de realizar estos cambios marcado declarativo de la nueva página maestra anidada debe ser similar al siguiente:


[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

Crear una página de contenido denominada `Alternate.aspx` en el `NestedMasterPages` carpeta y enlazarlo a la `SimpleNestedAlternate.master` página maestra anidada. Agregue el texto, "Hello, from alternativo!" en el control de contenido que se corresponde con `MainContent`. La figura 7 muestra `Alternate.aspx` cuando se ven a través del Diseñador de Visual Studio.


[![Alternate.aspx está enlazado a la página maestra SimpleNestedAlternate.master](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**Figura 07**: `Alternate.aspx` está enlazado a la `SimpleNestedAlternate.master` página maestra ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image21.png))


Compare el diseñador en la figura 7 hasta el diseñador en la figura 6. Ambas páginas de contenido comparten el mismo diseño definido en la página principal de nivel superior (`Simple.master`), es decir, el título "Tutorial de las páginas de la maestra anidada (Simple)". Todavía tienen distinto contenido definido en sus páginas maestras primaria: el texto "Hello, from SimpleNested!" en la figura 6 y "Hello, from SimpleNestedAlternate!" en la figura 7. Concedido, estas diferencias aquí son triviales, pero puede ampliar este ejemplo para incluir las diferencias más significativas. Por ejemplo, el `SimpleNested.master` página podría incluir un menú con opciones específicas de las páginas de contenido, mientras que `SimpleNestedAlternate.master` podría tener la información relativa a las páginas de contenido que se enlazan a él.

Ahora, imagine que es necesario realizar un cambio en el diseño general del sitio. Por ejemplo, imagine que queremos agregar una lista de vínculos comunes a todas las páginas de contenido. Para lograr esto, actualice la página principal de nivel superior, `Simple.master`. Los cambios se reflejan inmediatamente en sus páginas maestras anidadas y, por extensión, sus páginas de contenido.

Para demostrar la facilidad con la que se puede cambiar el diseño general del sitio, abra el `Simple.master` página principal y agregue el marcado siguiente entre las `topContent` y `mainContent` `<div>` elementos:


[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

Esto agrega dos vínculos a la parte superior de cada página que se enlaza a `Simple.master`, `SimpleNested.master`, o `SimpleNestedAlternate.master`; estos cambios se aplican a las páginas maestras anidadas todo y sus páginas de contenido inmediatamente. La figura 8 muestra `Alternate.aspx` cuando se ve mediante un explorador. Tenga en cuenta la adición de los vínculos en la parte superior de la página (en comparación con la figura 7).


[![Puede cambiar a la página principal de nivel superior son reflejan inmediatamente en su anidados páginas maestras y sus páginas de contenido](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**Figura 08**: cambiar a la página principal de nivel superior son reflejan inmediatamente en su anidados páginas maestras y sus páginas de contenido ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Uso de una página maestra anidada para la sección de administración

En este momento se ha examinado las ventajas de maestras anidadas páginas y ha visto cómo crear y usarlos en una aplicación ASP.NET. Los ejemplos en los pasos 1, 2 y 3, sin embargo, implicaba la creación de una nueva página principal de nivel superior, nuevas páginas maestras anidadas y nuevas páginas de contenido. ¿Agregar una nueva página maestra anidada en un sitio Web con una página principal de nivel superior existente y las páginas de contenido?

Integración de una página maestra anidada en un sitio Web existente y asociarla a las páginas de contenido existentes requieren un poco más esfuerzo que comenzar desde cero. Los pasos 4, 5, 6 y 7, exploración estos desafíos como aumentamos nuestra aplicación de demostración para incluir una nueva página maestra anidada denominada `AdminNested.master` que contiene instrucciones para el administrador y se utiliza por las páginas ASP.NET en el `~/Admin` carpeta.

Integración de una página maestra anidada en nuestra aplicación de demostración presenta los siguientes obstáculos:

- El contenido existente de páginas en el `~/Admin` carpeta tiene ciertas expectativas de su página maestra. Para empezar, esperan que ciertos controles ContentPlaceHolder estén presentes. Además, el `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` público de la página principal de llamar a las páginas `RefreshRecentProductsGrid` método, establece su `GridMessageText` propiedad, o tiene un controlador de eventos para su `PricesDoubled` eventos. Por lo tanto, nuestra página maestra anidada debe proporcionar el mismo ContentPlaceHolders y miembros públicos.
- En el tutorial anterior se ha mejorado la `BasePage` clase para establecer dinámicamente el `Page` del objeto `MasterPageFile` propiedad basándose en una variable de sesión. ¿Cómo a admitimos páginas maestra dinámicas al usar páginas maestras anidadas?

Se producirá un error en estos dos desafíos cuando se cree la página maestra anidada y usarlo desde nuestras páginas de contenido existentes. Comenzaremos investigar y superar estos problemas a medida que surjan.

## <a name="step-4-creating-the-nested-master-page"></a>Paso 4: Creación de la página maestra anidada

Nuestra primera tarea consiste en crear la página maestra anidada que va a usar las páginas en la sección de administración. Como vimos en el paso 2, cuando agrega un nuevo anidados página maestra necesitamos especificar la página principal de la página maestra anidada primaria. Pero tenemos dos páginas principales de nivel superior: `Site.master` y `Alternate.master`. Recuerde que hemos creado `Alternate.master` en el tutorial anterior y ha escrito el código el `BasePage` clase que establece el objeto de página `MasterPageFile` propiedad en tiempo de ejecución como `Site.master` o `Alternate.master` dependiendo del valor de la `MyMasterPage` Variable de sesión.

¿Cómo se configura nuestra página maestra anidada para que utilice la página principal de nivel superior adecuada? Tenemos dos opciones:

- Cree dos páginas maestras anidadas, `AdminNestedSite.master` y `AdminNestedAlternate.master`y se enlazan a las páginas principales de nivel superior `Site.master` y `Alternate.master`, respectivamente. En `BasePage`, a continuación, se establecería el `Page` del objeto `MasterPageFile` a la página maestra anidada adecuada.
- Crear una sola página maestra anidada y hacer que las páginas de contenido usan esta página principal determinada. Entonces, en tiempo de ejecución, sería necesario establecer la página maestra anidada `MasterPageFile` propiedad a la página maestra nivel superior adecuada en tiempo de ejecución. (Ya que es posible que haya descubierto en este momento, las páginas maestras también tienen un `MasterPageFile` propiedad.)

Vamos a usar la segunda opción. Crear un único archivo de página maestra anidada en la `~/Admin` carpeta denominada `AdminNested.master`. Dado que ambos `Site.master` y `Alternate.master` tienen el mismo conjunto de controles ContentPlaceHolder, no importa qué página maestra enlaza, aunque lo animo a enlazarlo a `Site.master` para fines de la coherencia.


[![Agregue una página maestra anidada a la carpeta ~/Admin.](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**Figura 09**: agregar una página maestra anidada para el `~/Admin` carpeta. ([Haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image27.png))


Dado que la página maestra anidada se enlaza a una página maestra con cuatro controles ContentPlaceHolder, Visual Studio agrega cuatro controles de contenido para marcado inicial del archivo de página maestra anidada nueva. Como se hizo en los pasos 2 y 3, agregue un control ContentPlaceHolder en cada control de contenido, dándole el mismo nombre que el control ContentPlaceHolder del nivel superior de la página maestra. Agregue también el siguiente marcado al control de contenido que se corresponde con el `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

A continuación, defina el `instructions` de clase CSS en el `Styles.css` y `AlternateStyles.css` archivos CSS. Las siguientes reglas CSS que tengan el estilo de elementos HTML la `instructions` clase que se mostrará con un color de fondo amarillo claro y un borde negro sólido:


[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

Dado que este marcado se agregó a la página maestra anidada, sólo aparecerá en las páginas que usan esta página maestra anidada (es decir, las páginas en la sección de administración).

Después de realizar estas adiciones a la página maestra anidada, su marcado declarativo debe tener un aspecto similar al siguiente:


[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

Tenga en cuenta que cada control de contenido tiene un control ContentPlaceHolder y que los controles ContentPlaceHolder `ID` propiedades se asignan los mismos valores que los controles ContentPlaceHolder correspondientes en la página principal de nivel superior. Además, el marcado específico de la sección de administración aparece en el `MainContent` ContentPlaceHolder.

La figura 10 muestra el `AdminNested.master` página maestra anidada cuando se ven a través del Diseñador de Visual Studio. Puede ver las instrucciones que aparecen en el cuadro amarillo en la parte superior de la `MainContent` control de contenido.


[![La página maestra anidada amplía la página de principal de nivel superior para incluir instrucciones para el administrador.](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**Figura 10**: la página maestra anidada amplía la página de principal de nivel superior para incluir instrucciones para el administrador. ([Haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Paso 5: Actualización de las páginas de contenido existentes para usar la nueva página maestra anidada

Cada vez que se agregue una nueva página de contenido a la sección de administración, debemos enlazar a la `AdminNested.master` página maestra que acabamos de crear. Pero ¿qué sucede con las existentes páginas de contenido? Actualmente, todas las páginas de contenido en el sitio se derivan de la `BasePage` (clase), que se establece mediante programación la página principal de la página de contenido en tiempo de ejecución. Esto no es el comportamiento que queremos para las páginas de contenido en la sección de administración. En su lugar, queremos que estas páginas de contenido para usar siempre el `AdminNested.master` página. Es responsabilidad de la página maestra anidada para elegir la página de contenido de nivel superior derecha en tiempo de ejecución.

Para la mejor forma de lograr esto deseada comportamiento consiste en crear una nueva clase de página base personalizada denominada `AdminBasePage` que abarca el `BasePage` clase. `AdminBasePage` a continuación, puede invalidar el `SetMasterPageFile` y establezca el `Page` del objeto `MasterPageFile` con el valor codificado de forma rígida "~ / Admin/AdminNested.master". De esta manera, cualquier página que se deriva de `AdminBasePage` usará `AdminNested.master`, mientras que cualquier página que se deriva de `BasePage` tendrá su `MasterPageFile` propiedad establece dinámicamente en "~ / Site.master" o "~ / Alternate.master" según el valor de la `MyMasterPage` Variable de sesión.

Empiece agregando un nuevo archivo de clase para el `App_Code` carpeta denominada `AdminBasePage.cs`. Tener `AdminBasePage` ampliar `BasePage` y, a continuación, reemplace el `SetMasterPageFile` método. En ese método, asigne el `MasterPageFile` el valor "~ / Admin/AdminNested.master". Después de realizar estos cambios en la clase de archivo debe ser similar al siguiente:


[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

Ahora necesitamos tener las páginas de contenido existentes en la administración de la sección derivan de `AdminBasePage` en lugar de `BasePage`. Vaya al archivo de clase de código subyacente para cada página de contenido en el `~/Admin` carpeta y realizar este cambio. Por ejemplo, en `~/Admin/Default.aspx` cambiaría la declaración de clase de código subyacente de:


[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

A:


[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

Figura 11 se muestra cómo la página principal de nivel superior (`Site.master` o `Alternate.master`), la página maestra anidada (`AdminNested.master`), y las páginas de contenido de sección de administración se relacionan entre sí.


[![La página maestra anidada define contenido determinado a las páginas en la sección de administración](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**Figura 11**: The anidados Master página define contenido específico a las páginas en la sección de administración ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Paso 6: Creación de reflejo de propiedades y métodos públicos de la página maestra

Recuerde que el `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` páginas interactúan mediante programación con la página maestra: `~/Admin/AddProduct.aspx` llama a la página principal del pública `RefreshRecentProductsGrid` método y establece su `GridMessageText` propiedad; `~/Admin/Products.aspx` tiene un controlador de eventos para el `PricesDoubled` eventos. En el tutorial anterior, creamos un abstracto `BaseMasterPage` clase que define estos miembros públicos.

El `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` páginas se suponen que su página principal se deriva de la `BaseMasterPage` clase. El `AdminNested.master` página, sin embargo, actualmente amplía la `System.Web.UI.MasterPage` clase. Como resultado, cuando se visita `~/Admin/Products.aspx` una `InvalidCastException` se inicia con el mensaje: "no se puede convertir el objeto de tipo ' ASP.admin\_adminnested\_maestra ' al tipo 'BaseMasterPage'."

Para solucionarlo necesitamos tener la `AdminNested.master` extender la clase de código subyacente `BaseMasterPage`. Actualice la declaración de clase de código subyacente de la página maestra anidada desde:


[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

A:


[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

No hemos terminado aún. Dado que el `BaseMasterPage` clase es abstracta, tenemos que reemplazar el `abstract` miembros, `RefreshRecentProductsGrid` y `GridMessageText`. Estos miembros se usan por las páginas principales de nivel superior para actualizar sus interfaces de usuario. (En realidad, solo el `Site.master` página maestra usa estos métodos, aunque ambas páginas principales de nivel superior implementan estos métodos, ya que ambos amplían `BaseMasterPage`.)

Aunque es necesario implementar estos miembros en `AdminNested.master`, todas estas implementaciones necesitan hacer es llamar simplemente el mismo miembro en la página principal de nivel superior utilizada por la página maestra anidada. Por ejemplo, cuando una página de contenido en la sección Administración llama a la página maestra anidada `RefreshRecentProductsGrid` método todo la página maestra anidada que necesita hacer es, a su vez, llamar a `Site.master` o `Alternate.master`del `RefreshRecentProductsGrid` método.

Para lograr esto, empiece por agregar lo siguiente `@MasterType` la directiva a la parte superior de `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

Recuerde que el `@MasterType` directiva agrega una propiedad fuertemente tipados a la clase de código subyacente denominada `Master`. A continuación, invalide el `RefreshRecentProductsGrid` y `GridMessageText` miembros y simplemente delegar la llamada a la `Master`correspondiente del método:


[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

Con este código en su lugar, debe ser capaz de visitar y use las páginas de contenido de la sección de administración. Figura 12 se muestra el `~/Admin/Products.aspx` página cuando se ve mediante un explorador. Como puede ver, la página incluye el cuadro de instrucciones de administración, que se define en la página maestra anidada.


[![Las páginas de contenido en la sección de administración incluyen instrucciones que aparecen en la parte superior de cada página](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**Figura 12**: las páginas de contenido en la administración sección incluye instrucciones en la parte superior de cada página ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Paso 7: Uso de la página principal de nivel superior adecuados en tiempo de ejecución

Mientras que todas las páginas de contenido en la sección de administración son totalmente funcionales, que todos usan la misma página principal de nivel superior y omitir la página principal seleccionada por el usuario en `ChooseMasterPage.aspx`. Este comportamiento es debido al hecho de que la página maestra anidada tiene su `MasterPageFile` propiedad se establece estáticamente en `Site.master` en su `<%@ Master %>` directiva.

Para usar la página principal de nivel superior seleccionada por el usuario final, es necesario establecer la `AdminNested.master`del `MasterPageFile` propiedad al valor de la `MyMasterPage` variable de sesión. Porque hemos establecido las páginas de contenido `MasterPageFile` propiedades en `BasePage`, es posible que piense que se establecería en la página maestra anidada `MasterPageFile` propiedad en `BaseMasterPage` o en el `AdminNested.master`de clase de código subyacente. Sin embargo, esto no funcionará porque tenemos que ha establecido la `MasterPageFile` propiedad al final de la fase de PreInit. La primera hora que podemos aprovechar mediante programación el ciclo de vida de la página de una página maestra es la etapa de Init (que se produce después de la fase de PreInit).

Por lo tanto, es necesario establecer la página maestra anidada `MasterPageFile` propiedad desde las páginas de contenido. El único contenido las páginas que usen el `AdminNested.master` página maestra que se derivan de `AdminBasePage`. Por lo tanto, podemos poner esta lógica no existe. En el paso 5 se anuló la `SetMasterPageFile` método, estableciendo el `Page` del objeto `MasterPageFile` propiedad en "~ / Admin/AdminNested.master". Actualización `SetMasterPageFile` establecer también la página maestra `MasterPageFile` propiedad para el resultado almacenado en la sesión:


[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

El `GetMasterPageFileFromSession` método, que se ha agregado a la `BasePage` clase en el tutorial anterior, devuelve la ruta de acceso de la página principal adecuada según el valor de la variable de sesión.

Con este cambio en su lugar, selección de página principal del usuario se traslada a la sección de administración. Figura 13 se muestra la misma página que figura 12, pero después de que el usuario ha cambiado su selección de página maestra para `Alternate.master`.


[![La página de administración anidada usa la página de principal de nivel superior seleccionado por el usuario](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**Figura 13**: la página de administración Nested utiliza el nivel superior principal página seleccionada por el usuario ([haga clic aquí para ver imagen en tamaño completo](nested-master-pages-cs/_static/image39.png))


## <a name="summary"></a>Resumen

Mucho like páginas cómo contenido pueden enlazar a una página maestra, es posible crear anidada páginas maestras por tener una página maestra secundaria enlazan a una página maestra principal. La página maestra secundaria puede definir controles de contenido para cada uno de ContentPlaceHolders sus padres; a continuación, puede agregar sus propios controles ContentPlaceHolder (así como otro elemento de marcado) a estos controles de contenido. Páginas maestras anidadas son muy útiles en las aplicaciones web de gran tamaño que todas las páginas compartan una apariencia y comportamiento general, pero determinadas secciones del sitio requieren personalizaciones únicas.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Páginas maestras de ASP.NET anidadas](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Sugerencias para páginas maestras anidadas y tiempo de diseño de VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 anidados de soporte técnico de página maestra](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [ *Sams Teach usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](specifying-the-master-page-programmatically-cs.md)
> [Siguiente](creating-a-site-wide-layout-using-master-pages-vb.md)
