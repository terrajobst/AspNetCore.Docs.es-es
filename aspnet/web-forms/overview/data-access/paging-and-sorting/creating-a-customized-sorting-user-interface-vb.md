---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: Creación de una interfaz de usuario de ordenación personalizada (VB) | Microsoft Docs
author: rick-anderson
description: Al mostrar una larga lista de los datos ordenados, puede ser muy útil para agrupar los datos relacionados mediante la introducción de las filas de separador. En este tutorial veremos cómo cre...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e94bbda0ca89b409515db1e223637e3a554cd31
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835873"
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>Creación de una interfaz de usuario de ordenación personalizada (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) o [descargar PDF](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Al mostrar una larga lista de los datos ordenados, puede ser muy útil para agrupar los datos relacionados mediante la introducción de las filas de separador. En este tutorial, veremos cómo crear este tipo una interfaz de usuario de ordenación.


## <a name="introduction"></a>Introducción

Cuando se muestra una lista larga de datos ordenados donde hay solo un conjunto de valores diferentes en la columna ordenada, un usuario final resultará difícil distinguir dónde, exactamente, se producen los límites de diferencia. Por ejemplo, hay 81 productos en la base de datos, pero solo nueve opciones de categoría diferentes (ocho categorías exclusivas más el `NULL` opción). Considere el caso de un usuario que está interesado en Examinar los productos que se encuentran en la categoría Mariscos. Desde una página que enumera *todas* de los productos en un único GridView, el usuario puede decidir es su mejor opción Ordenar los resultados por categoría, que se vayan a agrupar todos los productos mariscos juntos. Después de ordenar por categoría, el usuario, a continuación, necesita buscando a través de la lista, buscando donde los productos agrupados por mariscos empezar y terminar. Puesto que los resultados se ordenan alfabéticamente por el nombre de categoría buscar los productos mariscos no es difícil, pero todavía requiere examinar detenidamente la lista de elementos de la cuadrícula.

Para ayudar a resaltar los límites entre grupos ordenados, muchos sitios Web emplean una interfaz de usuario que agrega un separador entre los grupos de este tipo. Separadores, como los que se muestran en la figura 1 permite que un usuario más rápido buscar un grupo determinado e identificar sus límites, así como determinar qué grupos distintos que existen en los datos.


[![Cada grupo de categorías es claramente identificado](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Figura 1**: cada grupo de categorías es claramente identifica ([haga clic aquí para ver imagen en tamaño completo](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


En este tutorial, veremos cómo crear este tipo una interfaz de usuario de ordenación.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Paso 1: Crear un control GridView estándar, que se puede ordenar

Antes, exploramos cómo aumentar el control GridView para proporcionar la interfaz de ordenación mejorada, permiten s en primer lugar cree un control GridView estándar, que se puede ordenar que se enumera los productos. Comience abriendo la `CustomSortingUI.aspx` página en el `PagingAndSorting` carpeta. Agregue un control GridView a la página, establezca su `ID` propiedad `ProductList`y enlazarlo a un nuevo origen ObjectDataSource. Configurar el origen ObjectDataSource para usar el `ProductsBLL` clase s `GetProducts()` método de selección de registros.

A continuación, configure el control GridView tal que solo contiene el `ProductName`, `CategoryName`, `SupplierName`, y `UnitPrice` BoundFields y CheckBoxField discontinuo. Por último, configure el control GridView para admitir la ordenación activando la casilla Habilitar la ordenación en la etiqueta inteligente de GridView s (o estableciendo su `AllowSorting` propiedad `true`). Después de realizar estas adiciones a la `CustomSortingUI.aspx` página, el marcado declarativo debe ser similar al siguiente:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Dedique un momento para ver nuestro progreso hasta ahora en un explorador. Figura 2 muestra el control GridView que se puede ordenar cuando sus datos se ordenan por categoría en orden alfabético.


[![Las operaciones de asignación GridView que se puede ordenar los datos se ordenan por categoría](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Figura 2**: s GridView que se puede ordenar los datos se ordenan por categoría ([haga clic aquí para ver imagen en tamaño completo](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Paso 2: Explorar las técnicas para agregar las filas de separador

Con genérico, que se puede ordenar GridView completa, todo lo que queda es que pueda agregar las filas de separador de GridView antes de cada grupo ordenado único. Pero, ¿cómo se pueden insertar esas filas en GridView? Básicamente, nos necesitamos para recorrer en iteración las filas de s GridView, determinar dónde se producen las diferencias entre los valores de la columna ordenada y, a continuación, agregar la fila de separación adecuados. Al pensar en este problema, parece natural que la solución se encuentra en alguna parte de las operaciones de asignación GridView `RowDataBound` controlador de eventos. Como se explicó en la [formato basado en datos personalizados](../custom-formatting/custom-formatting-based-upon-data-vb.md) tutorial, normalmente se usa este controlador de eventos al aplicar formato en el nivel de fila en función de los datos de fila s. Sin embargo, el `RowDataBound` controlador de eventos no es la solución en este caso, cuando no se agregan filas a la GridView mediante programación desde este controlador de eventos. Las operaciones de asignación GridView `Rows` colección, de hecho, es de solo lectura.

Para agregar filas adicionales en el control GridView, tenemos tres opciones:

- Agregue estas filas de separador de metadatos a los datos reales que está enlazados el control GridView
- Después de que el control GridView se ha enlazado a los datos, agregar más `TableRow` controlan la recolección de instancias para la s de GridView
- Crear un control de servidor personalizado que extiende el control GridView e invalida los métodos responsables de construir la estructura de s GridView

Crear un control de servidor personalizado sería el mejor enfoque si se necesita esta funcionalidad en muchas páginas web o a través de varios sitios Web. Sin embargo, implicaría un poco de código y una exploración exhaustiva las profundidades de los funcionamientos internos de s GridView. Por lo tanto, no se considerará esa opción para este tutorial.

Las otras dos opciones de agregar filas de separador para los datos reales que se va a enlazar a GridView y manipular la colección de controles GridView s después de su estado enlazado: atacar el problema de forma diferente y merecen una explicación.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Agregar filas a los datos enlazados en el control GridView

Cuando el control GridView se enlaza a un origen de datos, crea un `GridViewRow` para cada registro devuelto por el origen de datos. Por lo tanto, nos podemos insertar las filas de separador necesario mediante la adición de registros de separador al origen de datos antes de enlazarla a la GridView. Figura 3 ilustra este concepto.


![Una técnica consiste en Agregar filas de separador para el origen de datos](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Figura 3**: una técnica consiste en Agregar filas de separador para el origen de datos


Usar los registros de separador de términos entre comillas porque no hay ningún registro de separación especial; en su lugar, nos debemos que algún modo Marcar que un registro determinado en el origen de datos actúa como un separador en lugar de una fila de datos normal. Para nuestros ejemplos, hemos re enlace un `ProductsDataTable` instancia en el control GridView, que se compone de `ProductRows`. Se puede marcar un registro como una fila de separación estableciendo su `CategoryID` propiedad `-1` (ya que este tipo un valor existe normalmente).

Para utilizar esta técnica d es necesario realizar los pasos siguientes:

1. Recuperar mediante programación los datos que se va a enlazar a GridView (un `ProductsDataTable` instancia)
2. Ordenar los datos según la s GridView `SortExpression` y `SortDirection` propiedades
3. Recorrer en iteración el `ProductsRows` en el `ProductsDataTable`, que buscan dónde se encuentran las diferencias en la columna ordenada
4. En cada límite de grupo, inserte un registro de separación `ProductsRow` instancia en la tabla de datos, uno que lo tiene s `CategoryID` establecido en `-1` (o cualquier designación se decidirá para marcar un registro como registro separador)
5. Después de insertar las filas de separador, enlazar mediante programación los datos en GridView

Además de estos cinco pasos, d también es necesario proporcionar un controlador de eventos para el s GridView `RowDataBound` eventos. En este caso, d comprobamos cada `DataRow` y determinar si fue un separador de fila, uno cuyo `CategoryID` configuración era `-1`. Si es así, d probablemente queremos ajustar el texto mostrado en las celdas o su formato.

Con esta técnica para insertar el grupo de límites de ordenación requiere un poco más trabajo que se ha descrito anteriormente, ya que deba proporcionar también un controlador de eventos para el s GridView `Sorting` eventos y mantener un seguimiento de la `SortExpression` y `SortDirection` valores.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipular la s GridView Control colección después de él s se ha enlazado a datos

En lugar de los datos de mensajería antes de enlazarla a la GridView, podemos agregar las filas de separador *después* los datos se ha enlazado a la GridView. El proceso de enlace de datos se crea la jerarquía de controles GridView s, que en realidad es simplemente un `Table` instancia se compone de una colección de filas, cada uno de los cuales se compone de una colección de celdas. En concreto, contiene la colección de controles GridView s un `Table` objeto en su raíz, un `GridViewRow` (que se deriva el `TableRow` clase) para cada registro en el `DataSource` enlazar a GridView y un `TableCell` objeto en cada `GridViewRow` instancia para cada campo de datos en el `DataSource`.

Para agregar filas separador entre cada grupo de ordenación, se puede manipular directamente esta jerarquía de control una vez que se ha creado. Que podemos estar seguros de que se ha creado la jerarquía de controles GridView s por última vez en el momento en que se va a representar la página. Por lo tanto, este método invalida el `Page` clase s `Render` método, momento en que la jerarquía de control final s GridView se actualiza para incluir las filas de separador necesario. Figura 4 ilustra este proceso.


[![Una técnica alternativa manipula la jerarquía de controles GridView s](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Figura 4**: una técnica alternativa manipula la jerarquía de controles de GridView s ([haga clic aquí para ver imagen en tamaño completo](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


Para este tutorial, usaremos este último enfoque para personalizar la experiencia de usuario de ordenación.

> [!NOTE]
> El código m se presenta en este tutorial se basa en el ejemplo proporcionado en [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) entrada de blog sobre s [jugar un poco con agrupación de ordenación de GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Paso 3: Agregar las filas de separador para la jerarquía de controles GridView s

Puesto que sólo deseamos agregar las filas de separador para la jerarquía de controles GridView s después de haber creado su jerarquía de controles y creado por última vez en esa página de visita, deseamos realizar esta adición al final del ciclo de vida de página, pero antes de la c real de GridView jerarquía ontrol se ha representado en HTML. El último punto posible en el que podemos lograr esto es el `Page` clase s `Render` evento, que es posible invalidar en nuestra clase de código subyacente mediante la firma del método siguiente:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Cuando el `Page` clase s original `Render` se invoca el método `base.Render(writer)` cada uno de los controles en la página se representará, generar el marcado en función de su jerarquía de controles. Por lo tanto, es imperativo que ambos llamamos `base.Render(writer)`, de modo que representa la página, y que se manipulan la s GridView control jerarquía antes de llamar a `base.Render(writer)`, de modo que se han agregado a la jerarquía de controles GridView s antes de que las filas de separador s sido representa.

Para insertar los encabezados de grupo Ordenar primero es necesario para asegurarse de que el usuario ha solicitado que se ordenan los datos. De forma predeterminada, el contenido de GridView s no se ordena y, por lo tanto, no queremos t necesidad de escribir cualquier grupo de ordenación de los encabezados.

> [!NOTE]
> Si desea que el control GridView se ordene por una columna en particular cuando se carga la página por primera vez, llamar a la GridView `Sort` método en la primera visita de página (pero no en los postbacks subsiguientes). Para ello, agregue esta llamada en el `Page_Load` controlador de eventos dentro de un `if (!Page.IsPostBack)` condicional. Vuelva a consultar el [paginación y ordenación de datos de informe](paging-and-sorting-report-data-vb.md) información tutorial para obtener más información sobre la `Sort` método.


Suponiendo que los datos se han ordenado, la siguiente tarea consiste en determinar qué columnas se ordenan los datos y, a continuación, para examinar las filas que se busca las diferencias en esa columna s valores. El código siguiente asegura que los datos se han ordenado y busca la columna por la que se han ordenado los datos:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

Si tiene el control GridView aún ser ordenada, la s GridView `SortExpression` propiedad no habrá se ha establecido. Por lo tanto, solo queremos agregar las filas de separador si esta propiedad tiene algún valor. Si es así, a continuación se debe determinar el índice de la columna por la que se ordenan los datos. Esto se logra mediante un bucle a través de las operaciones de asignación GridView `Columns` colección, búsqueda de la columna cuya propiedad `SortExpression` propiedad es igual a la s GridView `SortExpression` propiedad. Además del índice de columna s también obtenemos el `HeaderText` propiedad, que se utiliza al mostrar las filas de separador.

Con el índice de la columna por la que se ordenan los datos, el último paso es enumerar las filas del control GridView. Para cada fila es necesario determinar si el valor de columna ordenada s difiere la anterior fila s ordenados s del valor de columna. Si es así, es necesario insertar un nuevo `GridViewRow` instancia en la jerarquía de controles. Esto se logra con el código siguiente:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Este código se inicia haciendo mediante programación el `Table` objeto encontrado en la raíz de la jerarquía de controles GridView s y la creación de una variable de cadena denominada `lastValue`. `lastValue` se utiliza para comparar el valor de columna s ordenados de fila actual con el valor de fila s anterior. A continuación, la s GridView `Rows` se enumera la colección y para cada fila se almacena el valor de la columna ordenada en el `currentValue` variable.

> [!NOTE]
> Para determinar el valor de la columna de una fila determinada s ordenados utilizo la celda s `Text` propiedad. Esto funciona bien para BoundFields, pero no se funcionan según sea necesario para TemplateFields, CheckBoxFields y así sucesivamente. Echemos un vistazo cómo para tener en cuenta los campos de GridView alternativos en breve.


El `currentValue` y `lastValue` , a continuación, se comparan las variables. Si difieren, necesitamos agregar una nueva fila de separación para la jerarquía de controles. Esto se logra al determinar el índice de la `GridViewRow` en el `Table` objeto s `Rows` colección, crear nuevas `GridViewRow` y `TableCell` instancias y, a continuación, agregar el `TableCell` y `GridViewRow` a la jerarquía de controles.

Tenga en cuenta que el separador de fila única s `TableCell` es un formato que abarca todo el ancho del control GridView, se ha formateado con el `SortHeaderRowStyle` clase CSS y tiene su `Text` , que muestra tanto en el grupo Ordenar nombre de propiedad (por ejemplo, la categoría) y el valor de s de grupo (por ejemplo, bebidas). Por último, `lastValue` se actualiza en el valor de `currentValue`.

La clase CSS que se usa para dar formato a la fila de encabezado de grupo ordenación `SortHeaderRowStyle` debe especificarse en el `Styles.css` archivo. No dude en usar cualquier configuración de estilo atractivo para usted. He usado la siguiente:


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

Con el código actual, la interfaz de ordenación agrega encabezados de grupo de ordenación al ordenar por cualquier BoundField (consulte la figura 5, que muestra una captura de pantalla al ordenar por el proveedor). Sin embargo, al ordenar por cualquier otro tipo de campo (por ejemplo, un CampoCasillaVerificación o TemplateField), los encabezados de grupo de ordenación son ningún lugar donde se encuentra (consulte la figura 6).


[![La interfaz de ordenación incluye los encabezados de grupo de ordenación al ordenar por BoundFields](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Figura 5**: la ordenación interfaz incluye ordenación grupo encabezados al ordenar por BoundFields ([haga clic aquí para ver imagen en tamaño completo](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![Los encabezados de grupo de ordenación son falta al ordenar una CampoCasillaVerificación](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Figura 6**: los encabezados de grupo de ordenación son falta al ordenar una CampoCasillaVerificación ([haga clic aquí para ver imagen en tamaño completo](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


La razón faltan los encabezados de grupo de ordenación al ordenar por un CampoCasillaVerificación es que el código actualmente usa simplemente el `TableCell` s `Text` propiedad para determinar el valor de la columna ordenada para cada fila. Para CheckBoxFields, el `TableCell` s `Text` propiedad es una cadena vacía; en su lugar, el valor está disponible a través de un control Web de casilla de verificación que se encuentra en la `TableCell` s `Controls` colección.

Para controlar los tipos de campo que no sean BoundFields, debemos aumentar el código donde la `currentValue` variable se asigna a comprobar la existencia de una casilla en la `TableCell` s `Controls` colección. En lugar de usar `currentValue = gvr.Cells(sortColumnIndex).Text`, reemplace este código por lo siguiente:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Este código examina la columna ordenada `TableCell` para la fila actual determinar si existen todos los controles en el `Controls` colección. Si hay, y el primer control es una casilla, la `currentValue` variable se establece en Sí o No, dependiendo de la casilla de verificación s `Checked` propiedad. En caso contrario, se toma el valor de la `TableCell` s `Text` propiedad. Esta lógica se puede replicar para controlar la ordenación para cualquier TemplateFields en el que puedan existir en el control GridView.

Con la adición de código anterior, los encabezados de grupo de ordenación están presentes cuando se ordena por el CampoCasillaVerificación suspendido (consulte la figura 7).


[![Los encabezados de grupo de ordenación están ahora presente al ordenar una CampoCasillaVerificación](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Figura 7**: los encabezados de grupo de ordenación están ahora presente al ordenar una CampoCasillaVerificación ([haga clic aquí para ver imagen en tamaño completo](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> Si tiene productos con `NULL` valores para la base de datos la `CategoryID`, `SupplierID`, o `UnitPrice` campos, esos valores se mostrarán como cadenas vacías en el control GridView de forma predeterminada, lo que significa que el texto de fila s separador para esos productos con `NULL`leerán los valores como categoría: (es decir, hay s sin nombre después de la categoría: al igual que con la categoría: bebidas). Si desea que un valor que se muestran aquí se puede establecer el BoundFields [ `NullDisplayText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) al texto que desea mostrar o puede agregar una instrucción condicional en el método Render al asignar el `currentValue` para el separador fila s `Text` propiedad.


## <a name="summary"></a>Resumen

El control GridView no incluye muchas opciones integradas para personalizar la interfaz de ordenación. Sin embargo, con un poco de código de bajo nivel, lo posible ajustar la jerarquía de controles GridView s para crear una interfaz más personalizada. En este tutorial hemos visto cómo agregar una fila de separador de grupo de ordenación para un control GridView que se puede ordenar, que se identifica más fácilmente los distintos grupos y esos límites de grupos. Para obtener ejemplos adicionales de interfaces de ordenación personalizadas, eche un vistazo [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [unos ASP.NET 2.0 GridView ordenación sugerencias y trucos](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) entrada de blog.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](sorting-custom-paged-data-vb.md)
