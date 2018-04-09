---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Anidar páginas maestras (VB) | Documentos de Microsoft
author: rick-anderson
description: Muestra cómo anidar una página maestra dentro de otra.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 8c0123c12bb653a7f680154e2155eae0eb129428
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="nested-master-pages-vb"></a>Páginas maestras anidadas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Muestra cómo anidar una página maestra dentro de otra.


## <a name="introduction"></a>Introducción

A lo largo de los últimos nueve tutoriales hemos visto cómo implementar un diseño de todo el sitio con páginas maestras. En pocas palabras, páginas maestras nos permiten, el desarrollador de páginas, para definir marcado comunes en la página maestra junto con regiones específicas que se pueden personalizar según una página de contenido a página contenido. Los controles ContentPlaceHolder en una página maestra indican las regiones personalizables; el formato personalizado para los controles ContentPlaceHolder se definen en la página de contenido a través de controles de contenido.

Las técnicas de página maestra que hemos analizado hasta ahora son ideales si dispone de un diseño único que se utiliza en todo el sitio. Sin embargo, muchos sitios Web grandes tiene un diseño de sitio que se personaliza a través de distintas secciones. Por ejemplo, considere una aplicación sanitaria usada por el personal de hospital para administrar la información del paciente, actividades y facturación. Puede haber tres tipos de páginas web en esta aplicación:

- Páginas específicas de los miembros de personal donde los miembros del personal pueden actualizar disponibilidad, ver programaciones o tiempo de vacaciones de solicitudes.
- Páginas específicas de paciente donde los miembros del personal ver o edición la información de un paciente específico.
- Las páginas específicas de facturación donde contables revisión actual notificación Estados e informes financieros.

Todas las páginas pueden compartir un diseño común, por ejemplo, un menú en la parte superior y una serie de vínculos usados con frecuencia a lo largo de la parte inferior. Aunque los ejes personal, paciente y páginas específicas de facturación que necesite personalizar este diseño genérico. Por ejemplo, quizás todas las páginas específicas de personal deben incluir una lista de calendario y tareas que muestra el usuario ha iniciado sesión actualmente disponibilidad y las programaciones diarias. Quizás todas las páginas específicas del paciente se necesitan mostrar el nombre, la dirección y la información de seguros para el paciente cuya información se está editando.

Es posible crear tales diseños personalizados mediante *páginas maestras anidadas*. Para implementar el escenario anterior, se iniciaría mediante la creación de una página maestra que define el contenido de diseño, el menú y el pie de página de todo el sitio, con ContentPlaceHolders definir las regiones personalizables. A continuación, se crearía tres páginas maestras anidadas, uno para cada tipo de página web. Cada página maestra anidada tendría que definir el contenido entre el tipo de páginas de contenido que usan la página maestra. En otras palabras, la página maestra anidada para páginas de contenido específico de paciente incluiría marcado y la lógica de programación para mostrar información sobre el paciente que se está editando. Al crear una nueva página específica del paciente se enlazaría a esta página maestra anidada.

Este tutorial comienza resaltando las ventajas de las páginas maestras anidadas. A continuación, se muestra cómo crear y usar páginas maestras anidadas.

> [!NOTE]
> Las páginas maestras anidadas han sido posibles desde la versión 2.0 de .NET Framework. Sin embargo, Visual Studio 2005 no incluía compatibilidad en tiempo de diseño para las páginas maestras anidadas. Las buenas noticias son que Visual Studio 2008 le ofrece una experiencia de tiempo de diseño para las páginas maestras anidadas. Si está interesado en usar páginas maestras anidadas, pero se sigue usando Visual Studio 2005, visite [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog, [sugerencias para las páginas maestras anidadas en tiempo de diseño de VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Las ventajas de las páginas maestras anidadas

Muchos sitios Web tiene un diseño de sitio exhaustiva, así como más personalizados diseños específicos para determinados tipos de páginas. Por ejemplo, en nuestra aplicación de demostración web hemos creado una sección de administración rudimentaria (las páginas en el `~/Admin` carpeta). Actualmente, las páginas web en el `~/Admin` carpeta utilizan la misma página maestra como esas páginas no está en la sección de administración (; es decir, `Site.master` o `Alternate.master`, según la selección del usuario).

> [!NOTE]
> Por ahora, supongamos que nuestro sitio tiene solo una página maestra, `Site.master`. También se abordará mediante páginas maestras anidadas con dos (o más) de las páginas maestras a partir de "Usando un anidada maestra para la administración sección de página" más adelante en este tutorial.


Imagine que se nos pidió para personalizar el diseño de las páginas de administración para incluir información adicional o vínculos que no estarían presentes en otras páginas en el sitio. Hay cuatro técnicas para implementar este requisito:

1. Agregar manualmente la información específica de la administración y los vínculos a cada página de contenido en el `~/Admin` carpeta.
2. Actualización de la `Site.master` página maestra para incluir la información específica de la sección de administración y los vínculos y, a continuación, agregue código a la página maestra para mostrar u ocultar estas secciones en función de si una de las páginas de administración se visita.
3. Crear una nueva página maestra específicamente para la sección de administración, copie en el marcado de `Site.master`, agregue la información específica de la sección de administración y los vínculos y, a continuación, actualice las páginas de contenido en el `~/Admin` carpeta que desea utilizar este nuevo patrón página.
4. Crear una página maestra anidada que se enlaza a `Site.master` y tener las páginas de contenido en el `~/Admin` anidadas de carpeta, usar esta nueva página maestra. Esta página maestra anidada podría incluir solo la información adicional y vínculos específicos a las páginas de administración y no tenga que repetir el marcado ya definido en `Site.master`.

La primera opción es menos agradable ya. La razón fundamental del uso de páginas maestras es mover fuera de tener que copiar y pegar marcado común a las nuevas páginas ASP.NET manualmente. La segunda opción es aceptable, pero hace que la aplicación sea más difícil de mantener tal y como lo rellena las páginas maestras con marcado que se muestra solo ocasionalmente y requiere que los desarrolladores editando la página principal que se va a resolver este marcado como tienen que recordar cuando, exactamente, determinadas marcas se muestran frente a cuando está oculto. Este enfoque sería menos tenable como las personalizaciones de cada vez más tipos de páginas web necesarios para incluirse en esta página maestra única.

La tercera opción elimina el desorden y complejidad emite el apareció con la segunda opción. Sin embargo, principal inconveniente de la opción de tres consiste en que requiere que copie y pegue el diseño común de `Site.master` a la nueva página maestra para la sección específica de administración. Si posteriormente se decide cambiar el diseño de todo el sitio que se tienen que recordar cambiarlo en dos lugares.

La cuarta opción, páginas maestras anidadas, nos ofrecen el mejor de las opciones de segundo y terceros. La información de diseño de todo el sitio se mantiene en un archivo: la página principal de nivel superior - mientras el contenido específico de determinadas regiones se separarse en varios archivos.

Este tutorial comienza con un vistazo a la creación y uso de una página maestra anidada simple. Creamos una nueva página maestra nivel superior, dos páginas maestras anidadas y dos páginas de contenido. A partir de "Usando un anidados maestra para la administración sección de página", veremos actualizando nuestra arquitectura de página maestra existente para incluir el uso de las páginas maestras anidadas. En concreto, se crea una página maestra anidada y usarlo para incluir contenido personalizado adicional para las páginas de contenido en el `~/Admin` carpeta.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Paso 1: Crear una página principal de nivel superior sencilla

Crear a un patrón anidado basado en uno de lo master existente páginas y, a continuación, actualizar una página de contenido existente para usar esta nueva página maestra anidada en lugar de la página principal de nivel superior implica cierta complejidad porque las páginas de contenido existentes ya prevén determinados Controles de ContentPlaceHolder definidos en la página principal de nivel superior. Por lo tanto, la página maestra anidada también debe incluir los mismos controles ContentPlaceHolder con los mismos nombres. Además, nuestra aplicación de demostración determinado tiene dos páginas maestras (`Site.master` y `Alternate.master`) que se asignan dinámicamente a una página de contenido según las preferencias del usuario, lo que más se agrega a esta complejidad. Examinaremos actualizando la aplicación existente para usar páginas maestras anidadas más adelante en este tutorial, pero vamos a centrarnos primero en un sencillo anidadas ejemplo páginas maestras.

Cree una carpeta nueva denominada `NestedMasterPages` y, a continuación, agregue un nuevo archivo de página maestra a la carpeta denominada `Simple.master`. (Consulte la figura 1 para una captura de pantalla del explorador de soluciones después de han agregado esta carpeta y archivo.) Arrastre el `AlternateStyles.css` archivo de hoja de estilos en el Explorador de soluciones en el diseñador. Esto agrega un `<link>` elemento al archivo de hoja de estilos en el `<head>` elemento después de que la página maestra `<head>` marcado del elemento debe ser similar:


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

A continuación, agregue el siguiente marcado dentro del formulario Web de `Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Este marcado muestra un vínculo titulado "Nested las páginas maestras (Simple)" en la parte superior de la página en una fuente grande blanca sobre un fondo azul marino. Bajo el que el `MainContent` ContentPlaceHolder. La figura 1 muestra la `Simple.master` página maestra cuando se cargó en el Diseñador de Visual Studio.


[![La página maestra anidada define contenido específico a las páginas en la sección de administración](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Figura 01**: The anidados Master página define contenido específico a las páginas en la sección de administración ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Paso 2: Crear una página maestra anidada Simple

`Simple.master` contiene dos controles ContentPlaceHolder: el `MainContent` ContentPlaceHolder agregamos dentro del formulario Web junto con la `head` ContentPlaceHolder en la `<head>` elemento. Si deseáramos crear una página de contenido y enlazarlo a `Simple.master` la página de contenido tendría dos controles de contenido que hacen referencia a los dos ContentPlaceHolders. De forma similar, si se crea una página maestra anidada y enlácelo al `Simple.master` , a continuación, la página maestra anidada tendrá dos controles de contenido.

Vamos a agregar una nueva página maestra anidada para el `NestedMasterPages` carpeta denominada `SimpleNested.master`. Haga doble clic en el `NestedMasterPages` carpeta y elija Agregar nuevo elemento. Se abrirá el cuadro de diálogo Agregar nuevo elemento que se muestra en la figura 2. Seleccione el tipo de plantilla de página maestra y escriba el nombre de la nueva página maestra. Para indicar que la nueva página maestra debe ser una página maestra anidada, active la casilla "Seleccionar la página principal".

A continuación, haga clic en el botón Agregar. Se mostrará el mismo, seleccione un cuadro de diálogo de página maestra que se ven al enlazar una página de contenido a una página maestra (consulte la figura 3). Elija la `Simple.master` página maestra en la `NestedMasterPages` carpeta y haga clic en Aceptar.

> [!NOTE]
> Si ha creado el sitio Web ASP.NET utilizando el modelo de proyecto de aplicación Web en lugar del modelo de proyecto de sitio Web no verá la casilla de verificación "Seleccionar la página principal" en el cuadro de diálogo Agregar nuevo elemento que se muestra en la figura 2. Para crear una página maestra anidada al utilizar el modelo de proyecto de aplicación Web debe elegir la plantilla de página maestra anidada (en lugar de la plantilla de página maestra). Después de seleccionar la plantilla anidada página maestra y haga clic en Agregar, el mismo seleccionar una página maestra se abre el cuadro de diálogo que se muestra en la figura 3.


[![Compruebe el &quot;seleccionar la página maestra&quot; casilla de verificación para agregar una página maestra anidada](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Figura 02**: Active la casilla "Seleccionar la página maestra" para agregar una página maestra anidada ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image6.png))


[![Enlazar la página maestra anidada a la página principal Simple.master](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Figura 03**: enlazar la página maestra anidada para el `Simple.master` página maestra ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image9.png))


Marcado declarativo de la página maestra anidada, que se muestra a continuación, contiene dos controles de contenido que hacen referencia a dos controles de ContentPlaceHolder el nivel superior de la página maestra.


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Excepto para el `<%@ Master %>` directiva marcado declarativo de la página maestra anidada inicial es idéntico para el marcado que inicialmente se genera cuando se enlaza una página de contenido a la misma página principal de nivel superior. Al igual que una página de contenido `<%@ Page %>` directiva, la `<%@ Master %>` aquí directiva incluye una `MasterPageFile` atributo que especifica la página maestra primaria de la página maestra anidada. La diferencia principal entre la página maestra anidada y una página de contenido enlazados a la misma página de nivel superior principal es que la página maestra anidada puede incluir controles ContentPlaceHolder. Controles de la página maestra anidada ContentPlaceHolder definen las regiones donde las páginas de contenido pueden personalizar el marcado.

Actualizar esta página maestra anidada para que se muestre el texto "Hola, desde SimpleNested." en el control de contenido que se corresponde con el `MainContent` control ContentPlaceHolder.


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Después de realizar esta adición, guarde la página maestra anidada y, a continuación, agregue una nueva página de contenido a la `NestedMasterPages` carpeta denominada `Default.aspx`y enlácelo a la `SimpleNested.master` página maestra. Tras agregar esta página puede ser sorprenderá ver que no contiene ningún control de contenido (consulte la figura 4). Sólo puede tener acceso una página de contenido su *principal* principal ContentPlaceHolders de la página. `SimpleNested.master` no contiene todos los controles ContentPlaceHolder; por lo tanto, cualquier página de contenido enlazado a esta página principal no puede contener los controles de contenido.


[![La nueva página de contenido no contiene controles de contenido](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Figura 04**: el nuevo contenido página contiene sin controles de contenido ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image12.png))


Lo que debemos hacer es actualizar la página maestra anidada (`SimpleNested.master`) para incluir controles ContentPlaceHolder. Por lo general debe tener las páginas maestras anidadas para incluir un ContentPlaceHolder para cada ContentPlaceHolder definido por su página maestra primaria, lo que permitirá su página maestra secundaria o una página de contenido para trabajar con cualquiera de ContentPlaceHolder el nivel superior de la página maestra controles.

Actualización de la `SimpleNested.master` página maestra debe incluir un ContentPlaceHolder en sus dos controles de contenido. Proporcionar a los controles de ContentPlaceHolder el mismo nombre que el control ContentPlaceHolder a que su control de contenido hace referencia. Es decir, agregue un control ContentPlaceHolder denominado `MainContent` para el contenido del control en `SimpleNested.master` que hace referencia a la `MainContent` ContentPlaceHolder en `Simple.master`. Haga lo mismo en el control de contenido que hace referencia el `head` ContentPlaceHolder.

> [!NOTE]
> Aunque recomienda nomenclatura los controles ContentPlaceHolder en la página maestra anidada el mismo que el ContentPlaceHolders en la página principal de nivel superior, no es necesaria este simetría de nomenclatura. Puede proporcionar a los controles ContentPlaceHolder en la página maestra anidada cualquier nombre que quiera. Sin embargo, que resulte más sencillo recordar qué ContentPlaceHolders se corresponden con las regiones de la página si mi página principal de nivel superior y las páginas maestras anidadas utilizan los mismos nombres.


Después de realizar estas adiciones su `SimpleNested.master` marcado declarativo de la página principal debe ser similar al siguiente:


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Eliminar el `Default.aspx` de contenido de página que acabamos de crear y, a continuación, vuelva a agregarlo, enlace a la `SimpleNested.master` página maestra. Este tiempo de Visual Studio agrega dos controles de contenido a la `Default.aspx`, ahora hace referencia a la ContentPlaceHolders definido en `SimpleNested.master` (consulte la figura 6). Agregue el texto, "¡Hello, de Default.aspx!" en el contenido del control que hace referencia a `MainContent`.

Figura 5 se muestra las tres entidades implicadas aquí - `Simple.master`, `SimpleNested.master`, y `Default.aspx` - y cómo se relacionan entre sí. Como se muestra en el diagrama, la página maestra anidada implementa controles de contenido para ContentPlaceHolder de su elemento primario. Si estas regiones deben poder acceder a la página de contenido, la página maestra anidada debe agregar su propio ContentPlaceHolders a los controles de contenido.


[![Las páginas principales de nivel superior y anidados dictan el diseño de la página de contenido](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Figura 05**: las páginas principales de nivel superior y anidados dictan el diseño de la página de contenido ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image15.png))


Este comportamiento muestra cómo una página de contenido o página maestra solo está al corriente de su página maestra primaria. Este comportamiento también se indica mediante el Diseñador de Visual Studio. Figura 6 se muestra el diseñador para `Default.aspx`. Aunque el diseñador muestra claramente qué regiones son editables de la página de contenido y lo que no son las partes, no eliminar la ambigüedad de son qué regiones no editables de la página maestra anidada y cuáles son las regiones de la página principal de nivel superior.


[![El momento de la página contenido incluye controles de contenido para ContentPlaceHolders la página maestra anidada](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Figura 06**: el contenido página ahora incluye controles de contenido para ContentPlaceHolders el anidado de la página maestra ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Paso 3: Agregar una segunda Simple página maestra anidada

El beneficio de las páginas maestras anidadas es más evidente cuando hay varias páginas maestras anidadas. Para ilustrar esta ventaja, cree otra página maestra anidada en la `NestedMasterPages` carpeta; nombre de esta nueva anidado de página maestra `SimpleNestedAlternate.master` y enlazarla a la `Simple.master` página maestra. Agregar controles ContentPlaceHolder en dos controles de contenido de la página maestra anidada como hicimos en el paso 2. Agregar el texto, "Hola, desde SimpleNestedAlternate." en el control de contenido que se corresponde con el nivel superior de la página maestra `MainContent` ContentPlaceHolder. Después de realizar estos cambios marcado declarativo de la nueva página maestra anidada debe ser similar al siguiente:


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Crear una página de contenido denominada `Alternate.aspx` en el `NestedMasterPages` carpeta y enlazarla a la `SimpleNestedAlternate.master` una página maestra anidada. Agregue el texto, "Hola, desde alternativo." en el control de contenido que se corresponde con `MainContent`. La figura 7 muestra `Alternate.aspx` cuando se ven a través del Diseñador de Visual Studio.


[![Alternate.aspx se enlaza a la página principal de SimpleNestedAlternate.master](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Figura 07**: `Alternate.aspx` está enlazado a la `SimpleNestedAlternate.master` página maestra ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image21.png))


Compare el diseñador en la figura 7 al diseñador en la figura 6. Las páginas de contenido comparten el mismo diseño que se define en la página principal de nivel superior (`Simple.master`), es decir, el título "Nested Master páginas Tutorial (Simple)". Todavía tienen distinto contenido definido en sus páginas maestras primario - el texto "Hola, desde SimpleNested." en la figura 6 y "Hola, desde SimpleNestedAlternate." en la figura 7. Concede, estas diferencias aquí son triviales, pero puede ampliar este ejemplo para incluir las diferencias más significativas. Por ejemplo, el `SimpleNested.master` página podría incluir un menú con opciones específicas de las páginas de contenido, mientras que `SimpleNestedAlternate.master` tenga información relativa a las páginas de contenido que se enlazan a él.

Ahora, imagine que necesitábamos realizar un cambio en el diseño del sitio exhaustiva. Por ejemplo, imagine que desea agregar una lista de vínculos comunes a todas las páginas de contenido. Para ello se actualice la página principal de nivel superior, `Simple.master`. Los cambios se reflejan de inmediato en sus páginas maestras anidadas y, por extensión, sus páginas de contenido.

Para mostrar la facilidad con la que se puede cambiar el diseño del sitio exhaustiva, abra el `Simple.master` página principal y agregue el siguiente marcado entre la `topContent` y `mainContent` `<div>` elementos:


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Esto agrega dos vínculos a la parte superior de cada página que se enlaza a `Simple.master`, `SimpleNested.master`, o `SimpleNestedAlternate.master`; estos cambios se aplican inmediatamente a anidada todas las páginas maestras y sus páginas de contenido. La figura 8 muestra `Alternate.aspx` cuando se ven a través de un explorador. Tenga en cuenta la suma de los vínculos en la parte superior de la página (en comparación con la figura 7).


[![Cambia a la página principal de nivel superior son reflejan de inmediato en su anidar páginas maestras y sus páginas de contenido](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Figura 08**: cambia a la página principal de nivel superior son reflejan de inmediato en su anidar páginas maestras y sus páginas de contenido ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Uso de una página maestra anidada para la sección de administración

En este momento se han examinado las ventajas de maestras anidadas páginas y ha visto cómo crear y usarlos en una aplicación ASP.NET. Los ejemplos en los pasos 1, 2 y 3, sin embargo, implica crear una nueva página principal de nivel superior, nuevas páginas maestras anidadas y nuevas páginas de contenido. ¿Qué hay de agregar una nueva página maestra anidada en un sitio Web con una página principal de nivel superior existente y páginas de contenido?

Integrar una página maestra anidada en un sitio Web existente y asociarla a las páginas de contenido existentes requieren un poco más esfuerzo que hacerlo desde el principio. Los pasos 4, 5, 6 y 7 explorar estos retos, tal y como se alimentan nuestra aplicación de demostración para incluir una nueva página maestra anidada denominada `AdminNested.master` que contiene instrucciones para el administrador y se utiliza por las páginas ASP.NET en el `~/Admin` carpeta.

Integrar una página maestra anidada en nuestra aplicación de demostración, presenta los obstáculos siguientes:

- El contenido existente de páginas en el `~/Admin` carpeta tienen ciertas expectativas de su página maestra. Para empezar, espera que ciertos controles ContentPlaceHolder estén presentes. Además, el `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` público de la página principal de llamar a páginas `RefreshRecentProductsGrid` método, establece su `GridMessageText` propiedad, o tiene un controlador de eventos para su `PricesDoubled` eventos. Por lo tanto, nuestra página maestra anidada debe proporcionar el mismo ContentPlaceHolders y miembros públicos.
- En el tutorial anterior se ha mejorado la `BasePage` clase para establecer dinámicamente el `Page` del objeto `MasterPageFile` propiedad basados en una variable de sesión. ¿Cómo a admitimos páginas maestra dinámicas al usar páginas maestras anidadas?

Se producirá un error en estas dos desafíos a medida se crea la página maestra anidada y usarlo desde nuestras páginas de contenido existentes. Comenzaremos investigar y superar estos problemas que surjan.

## <a name="step-4-creating-the-nested-master-page"></a>Paso 4: Creación de la página maestra anidada

Nuestra primera tarea consiste en crear la página maestra anidada que va a usar las páginas en la sección de administración. Como vimos en el paso 2, cuando agrega un nuevo anidados página maestra es preciso especificar la página maestra primaria de la página maestra anidada. Pero tenemos dos páginas maestras de nivel superior: `Site.master` y `Alternate.master`. Recuerde que hemos creado `Alternate.master` en el tutorial anterior y escribió el código el `BasePage` clase ese conjunto el `Page` del objeto `MasterPageFile` propiedad en tiempo de ejecución como `Site.master` o `Alternate.master` dependiendo del valor de la `MyMasterPage` Variable de sesión.

¿Cómo configuro nuestra página maestra anidada para que utilice la página de nivel superior principal adecuada? Se tiene dos opciones:

- Crear dos páginas maestras anidadas, `AdminNestedSite.master` y `AdminNestedAlternate.master`y enlazarlas a las páginas principales de nivel superior `Site.master` y `Alternate.master`, respectivamente. En `BasePage`, a continuación, se recomienda establecer el `Page` del objeto `MasterPageFile` a la página maestra anidada adecuada.
- Crea una sola página maestra anidada y tiene las páginas de contenido usan esta página principal determinada. A continuación, en tiempo de ejecución, se necesita establecer la página maestra anidada `MasterPageFile` propiedad a la página maestra nivel superior adecuada en tiempo de ejecución. (Tal y como se podría haber imaginado hasta ahora, las páginas maestras también tienen un `MasterPageFile` propiedad.)

Vamos a usar la segunda opción. Crear un único archivo de página maestra anidada en la `~/Admin` carpeta denominada `AdminNested.master`. Dado que ambos `Site.master` y `Alternate.master` tienen el mismo conjunto de controles ContentPlaceHolder, no importa qué página maestra enlaza, aunque promueve a enlazar `Site.master` para simplificar de coherencia.


[![Agregar una página principal anidada a la carpeta ~/Admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Figura 09**: agregar una página maestra anidada para el `~/Admin` carpeta. ([Haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image27.png))


Dado que la página maestra anidada se enlaza a una página maestra con cuatro controles ContentPlaceHolder, Visual Studio agrega cuatro controles de contenido para marcado inicial del archivo de página maestra anidada nueva. Al igual que hicimos en los pasos 2 y 3, agregue un control ContentPlaceHolder en cada control de contenido, dándole el mismo nombre que el control de ContentPlaceHolder el nivel superior de la página maestra. También agregue el siguiente marcado para el control de contenido que se corresponde con el `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

A continuación, defina el `instructions` de clase CSS en los `Styles.css` y `AlternateStyles.css` archivos CSS. Las reglas CSS siguientes hacen elementos HTML con estilo con el `instructions` clase a se muestran con un color de fondo amarillo claro y un borde negro sólido:


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Porque se ha agregado este marcado para la página maestra anidada, sólo aparecerá en las páginas que usan esta página principal anidada (es decir, las páginas en la sección de administración).

Después de realizar estas adiciones a la página maestra anidada, su marcado declarativo debe ser similar al siguiente:


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Tenga en cuenta que cada control de contenido tiene un control ContentPlaceHolder y que los controles de ContentPlaceHolder `ID` propiedades se asignan los mismos valores que los controles ContentPlaceHolder correspondientes en la página principal de nivel superior. Además, el marcado específico de la sección de administración aparece en el `MainContent` ContentPlaceHolder.

La figura 10 muestra la `AdminNested.master` página maestra anidada cuando se ven a través del Diseñador de Visual Studio. Puede ver las instrucciones que aparecen en el cuadro amarillo en la parte superior de la `MainContent` control de contenido.


[![La página maestra anidada amplía la página de patrón de nivel superior para incluir instrucciones para el administrador.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Figura 10**: la página maestra anidada extiende la página de patrón de nivel superior para incluir instrucciones para el administrador. ([Haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Paso 5: Actualización de las páginas de contenido existentes para usar la nueva página maestra anidada

Cada vez que se agregue una nueva página de contenido a la sección de administración es necesario enlazarla a la `AdminNested.master` página maestra que acabamos de crear. Pero ¿qué ocurre con las existentes páginas de contenido? Actualmente, todas las páginas de contenido en el sitio se derivan de la `BasePage` (clase), que establece mediante programación el contenido de página principal en tiempo de ejecución. No es el comportamiento que deseamos para las páginas de contenido en la sección de administración. En su lugar, queremos que estas páginas de contenido que utilice siempre la `AdminNested.master` página. Será la responsabilidad de la página maestra anidada para elegir la página de contenido de nivel superior derecha en tiempo de ejecución.

Para la mejor manera de lograr esto deseada comportamiento consiste en crear una nueva clase de página base personalizada denominada `AdminBasePage` que abarca el `BasePage` clase. `AdminBasePage` a continuación, puede invalidar la `SetMasterPageFile` y establezca el `Page` del objeto `MasterPageFile` con el valor codificado de forma rígida "~ / Admin/AdminNested.master". De este modo, cualquier página que se deriva de `AdminBasePage` utilizará `AdminNested.master`, mientras que cualquier página que se deriva de `BasePage` tendrá su `MasterPageFile` propiedad establece dinámicamente en "~ / Site.master" o "~ / Alternate.master" en función del valor de la `MyMasterPage` Variable de sesión.

Empiece agregando un nuevo archivo de clase para la `App_Code` carpeta denominada `AdminBasePage.vb`. Tener `AdminBasePage` extender `BasePage` y, a continuación, reemplace el `SetMasterPageFile` método. En ese método asignar el `MasterPageFile` el valor "~ / Admin/AdminNested.master". Después de realizar estos cambios en la clase de archivo debe ser similar al siguiente:


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Ahora necesitamos tener las páginas de contenido existentes en la administración de la sección que se derivan de `AdminBasePage` en lugar de `BasePage`. Vaya al archivo de clase de código subyacente para cada página de contenido en el `~/Admin` carpeta y realizar este cambio. Por ejemplo, en `~/Admin/Default.aspx` se cambiaría la declaración de clase de código subyacente de:


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

A:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

Figura 11 muestra cómo la página principal de nivel superior (`Site.master` o `Alternate.master`), la página maestra anidada (`AdminNested.master`), y las páginas de contenido de sección de administración se relacionan entre sí.


[![La página maestra anidada define contenido específico a las páginas en la sección de administración](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Figura 11**: The anidados Master página define contenido específico a las páginas en la sección de administración ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Paso 6: Creación de reflejo de propiedades y métodos públicos de la página maestra

Recuerde que el `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` páginas interactúan mediante programación con la página maestra: `~/Admin/AddProduct.aspx` llamadas a la página principal del pública `RefreshRecentProductsGrid` método y establece su `GridMessageText` propiedad; `~/Admin/Products.aspx` tiene un controlador de eventos para el `PricesDoubled` eventos. En el tutorial anterior, creamos un `MustInherit` `BaseMasterPage` clase que define estos miembros públicos.

El `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` páginas suponga que su página maestra se deriva de la `BaseMasterPage` clase. El `AdminNested.master` página, sin embargo, actualmente extiende la `System.Web.UI.MasterPage` clase. Como resultado, cuando se visita `~/Admin/Products.aspx` una `InvalidCastException` se inicia con el mensaje: "no se puede convertir un objeto de tipo ' ASP.admin\_adminnested\_maestra ' al tipo 'BaseMasterPage'."

Para solucionar esto es necesario tener la `AdminNested.master` extender la clase de código subyacente `BaseMasterPage`. Actualice la declaración de clase de código subyacente de la página maestra anidada desde:


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

A:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

No terminamos todavía. Es necesario reemplazar los miembros marcados como `MustOverride`, es decir, `RefreshRecentProductsGrid` y `GridMessageText`. Estos miembros son utilizados por las páginas principales de nivel superior para actualizar sus interfaces de usuario. (En realidad, solo el `Site.master` página maestra usa estos métodos, aunque ambas páginas principales de nivel superior implementan estos métodos, ya que ambos amplían `BaseMasterPage`.)

Mientras se necesita implementar estos miembros en `AdminNested.master`, todas estas implementaciones necesitan hacer es simplemente llamar al mismo miembro en la página maestra nivel superior utilizada por la página maestra anidada. Por ejemplo, cuando una página de contenido en la sección Administración llama a la página de principal anidada `RefreshRecentProductsGrid` método todo la página maestra anidada lo que necesita hacer es, a su vez, llame a `Site.master` o `Alternate.master`del `RefreshRecentProductsGrid` método.

Para lograr esto, empiece por agregar la siguiente `@MasterType` directiva al principio del `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Recuerde que el `@MasterType` directiva agrega una propiedad fuertemente tipados a la clase de código subyacente denominada `Master`. A continuación, invalidar la `RefreshRecentProductsGrid` y `GridMessageText` miembros y simplemente delegar la llamada a la `Master`correspondiente del método:


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Con este código en su lugar, podrá visitar y use las páginas de contenido de la sección de administración. La figura 12 muestra la `~/Admin/Products.aspx` página cuando se ve mediante un explorador. Como puede ver, la página incluye el cuadro de instrucciones de administración, que se define en la página maestra anidada.


[![Las páginas de contenido en la sección de administración incluyen instrucciones en la parte superior de cada página](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Figura 12**: las páginas de contenido en la administración sección incluye instrucciones en la parte superior de cada página ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Paso 7: Uso de la página maestra de nivel superior apropiada en tiempo de ejecución

Aunque todas las páginas de contenido en la sección de administración son totalmente funcionales, se todos utilizan la misma página principal de nivel superior y omitir la página principal seleccionada por el usuario en `ChooseMasterPage.aspx`. Este comportamiento es debido al hecho de que la página maestra anidada tiene su `MasterPageFile` propiedad estáticamente establecida en `Site.master` en su `<%@ Master %>` directiva.

Para usar la página principal de nivel superior seleccionada por el usuario final, es necesario establecer la `AdminNested.master`del `MasterPageFile` en el valor en el `MyMasterPage` variable de sesión. Dado que hemos establecido las páginas de contenido `MasterPageFile` propiedades en `BasePage`, puede pensar que se establecería en la página maestra anidada `MasterPageFile` propiedad en `BaseMasterPage` o en el `AdminNested.master`de clase de código subyacente. Sin embargo, esto no funcionará porque necesitamos debe haber configurado el `MasterPageFile` propiedad hacia el final de la fase de PreInit. La primera hora que se puede pulsar mediante programación en el ciclo de vida de la página de una página maestra es la fase de inicialización (que se produce después de la fase de PreInit).

Por lo tanto, es necesario establecer la página maestra anidada `MasterPageFile` propiedad desde las páginas de contenido. Solo el contenido de las páginas que usan el `AdminNested.master` página maestra que se derivan de `AdminBasePage`. Por lo tanto, podemos colocamos esta lógica no existe. En el paso 5 se reemplazó el `SetMasterPageFile` método, el objeto de página de configuración `MasterPageFile` propiedad en "~ / Admin/AdminNested.master". Actualización `SetMasterPageFile` también de establecer la página maestra `MasterPageFile` propiedad para el resultado almacenado en la sesión:


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

El `GetMasterPageFileFromSession` método, que se agrega a la `BasePage` clase en el tutorial anterior, devuelve la ruta de acceso de archivo de página principal adecuada según el valor de la variable de sesión.

Con este cambio en su lugar, la selección del usuario página maestra se conserva en la sección de administración. Figura 13 muestra la misma página que figura 12, pero después de que el usuario ha cambiado su selección de página maestra para `Alternate.master`.


[![La página de administración anidada usa la página de principal de nivel superior seleccionado por el usuario](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Figura 13**: la página de administración de Nested utiliza el nivel superior Master página seleccionada por el usuario ([haga clic aquí para ver la imagen a tamaño completo](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>Resumen

Cantidad like páginas de administración de contenido pueden enlazar a una página maestra, es posible crear anidada páginas maestras debido a una página maestra secundaria enlazan a una página maestra primaria. La página maestra secundaria puede definir controles de contenido para cada uno de ContentPlaceHolders su elemento primario; a continuación, puede agregar sus propios controles ContentPlaceHolder (así como otro tipo de marcado) a estos controles de contenido. Páginas maestras anidadas son muy útiles en las aplicaciones web de gran tamaño donde todas las páginas comparten un apariencia y funcionamiento exhaustiva, pero determinadas secciones del sitio requieren personalizaciones únicas.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Páginas maestras ASP.NET anidadas](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Sugerencias para páginas maestras anidadas y tiempo de diseño de VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 anidados soporte de página maestra](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](specifying-the-master-page-programmatically-vb.md)
