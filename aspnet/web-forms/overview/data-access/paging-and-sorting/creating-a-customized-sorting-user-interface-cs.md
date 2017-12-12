---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: "Crear una interfaz de usuario personalizadas de ordenación (C#) | Documentos de Microsoft"
author: rick-anderson
description: "Al mostrar una lista larga de datos ordenados, puede resultar muy útil para agrupar los datos relacionados mediante la introducción de filas de separador. En este tutorial veremos cómo cre..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: bc009272df3626402ee4c52578f9b364f70a4e78
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-customized-sorting-user-interface-c"></a>Crear una interfaz de usuario personalizadas de ordenación (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) o [descarga de PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Al mostrar una lista larga de datos ordenados, puede resultar muy útil para agrupar los datos relacionados mediante la introducción de filas de separador. En este tutorial veremos cómo crear este tipo una interfaz de usuario de ordenación.


## <a name="introduction"></a>Introducción

Al mostrar una lista larga de datos ordenados que hay pocos valores distintos en la columna ordenada, un usuario final resultará difícil de discernir donde, exactamente, se producen los límites de diferencia. Por ejemplo, hay 81 productos en la base de datos, pero solo nueve opciones de categoría distinta (ocho categorías únicos además de la interfaz `NULL` opción). Considere el caso de un usuario que está interesado en el examen de los productos que se encuentran en la categoría Seafood. Desde una página que enumera *todos los* de los productos en un único GridView, el usuario podría decidir la mejor opción es ordenar los resultados por categoría, que se vayan a agrupar todos los productos de Seafood juntos. Después de ordenar por categoría, el usuario, a continuación, debe buscar a través de la lista, buscando donde los productos agrupados Seafood empezar y terminar. Dado que los resultados se ordenan alfabéticamente por el nombre de categoría buscar los productos Seafood no es difícil, pero seguirá necesitando estrechamente examinar la lista de elementos de la cuadrícula.

Con el fin de resaltar los límites entre grupos ordenados, muchos sitios Web emplea una interfaz de usuario que agrega un separador entre estos grupos. Separadores como las que se muestra en la figura 1 permite a más rápidamente encontrar un grupo determinado e identificar sus límites, así como determinar qué grupos distintos existen en los datos de un usuario.


[![Cada grupo de categorías es claramente identificado](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Figura 1**: cada grupo de categorías es claramente identificado ([haga clic aquí para ver la imagen a tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image3.png))


En este tutorial veremos cómo crear este tipo una interfaz de usuario de ordenación.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Paso 1: Crear un control GridView estándar, que se puede ordenar

Antes, exploramos cómo aumentar GridView para proporcionar la interfaz de ordenación mejorada, permiten s primero cree un control estándar, que se puede ordenar GridView que muestra los productos. Comience abriendo la `CustomSortingUI.aspx` página en el `PagingAndSorting` carpeta. Agregar un control GridView a la página, establezca su `ID` propiedad `ProductList`y enlazarla a un nuevo origen ObjectDataSource. Configurar el ObjectDataSource para usar el `ProductsBLL` clase s. `GetProducts()` método de selección de registros.

A continuación, configure el control GridView, que solo contenga el `ProductName`, `CategoryName`, `SupplierName`, y `UnitPrice` BoundFields y CheckBoxField suspendido. Por último, configurar el control GridView para admitir la ordenación activando la casilla de verificación Habilitar ordenación en la etiqueta inteligente de GridView s (o estableciendo su `AllowSorting` propiedad `true`). Después de realizar estas adiciones a la `CustomSortingUI.aspx` página, el marcado declarativo debe ser similar al siguiente:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

Tómese un momento para ver el progreso hasta el momento en un explorador. La figura 2 muestra que se puede ordenar GridView cuando sus datos se ordenan por categoría en orden alfabético.


[![Las operaciones de asignación GridView que se puede ordenar los datos se ordenan por categoría](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Figura 2**: s GridView que se puede ordenar los datos se ordenan por categoría ([haga clic aquí para ver la imagen a tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Paso 2: Explorar técnicas para agregar las filas de separador

Con genérico, que se puede ordenar GridView completando, todo lo que queda es para que pueda agregar las filas de separador en GridView antes de cada grupo ordenado único. Pero, ¿cómo se pueden insertar esas filas en GridView? Básicamente, se necesitamos para recorrer en iteración las filas de s GridView, determinar las diferencias entre los valores de la columna ordenada y, a continuación, agregue la fila de separación adecuados. Al pensar en este problema, parece que la solución se encuentre en alguna de las operaciones de asignación GridView natural `RowDataBound` controlador de eventos. Como se explicó en la [formato basado en datos personalizados](../custom-formatting/custom-formatting-based-upon-data-cs.md) tutorial, este controlador de eventos normalmente se usa para aplicar formato de nivel de fila basado en los datos de fila s. Sin embargo, la `RowDataBound` controlador de eventos no es la solución en este caso, cuando no se agregan filas a la GridView mediante programación desde este controlador de eventos. Las operaciones de asignación GridView `Rows` colección, de hecho, es de solo lectura.

Para agregar filas adicionales a la GridView tenemos tres opciones:

- Agregar estas filas de separador de metadatos a los datos reales que se enlaza a la GridView
- Después de que el control GridView se ha enlazado a los datos, agregar más `TableRow` instancias para las operaciones de asignación GridView controlan la recolección
- Crear un control de servidor personalizado que extiende el control GridView e invalida los métodos responsables de crear la estructura de s de GridView

Crear un control de servidor personalizado sería el mejor método si se necesita esta funcionalidad en muchas páginas web o a través de varios sitios Web. Sin embargo, implicaría una gran cantidad de código y una exploración exhaustiva a los niveles profundos de los mecanismos internos de s de GridView. Por lo tanto, no utilizaremos esta opción para este tutorial.

Las otras dos opciones de agregar filas de separador para los datos reales que se va a enlazar a GridView y manipular la colección de controles de GridView s después de su está enlazado: atacar el problema de forma diferente y merecen una explicación.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Agregar filas a los datos enlazados a GridView

Cuando el control GridView está enlazado a un origen de datos, se crea un `GridViewRow` por cada registro devuelto por el origen de datos. Por lo tanto, podemos inyectamos las filas de separador necesario mediante la adición de registros de separador al origen de datos antes de enlazar a GridView. Figura 3 ilustra este concepto.


![Una técnica implica agregar filas separador al origen de datos](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Figura 3**: una técnica implica agregar filas separador al origen de datos


Usar los registros de separador de términos entre comillas porque no hay ningún registro separador especial; en su lugar, se debemos que algún modo Marcar que un registro concreto en el origen de datos actúa como un separador en lugar de una fila de datos normal. Para nuestros ejemplos, se re enlace un `ProductsDataTable` instancia para el control GridView, que se compone de `ProductRows`. Nos tengamos que marca un registro como una fila de separación estableciendo su `CategoryID` propiedad `-1` (ya que este tipo t de ha de valor existe normalmente).

Para utilizar esta técnica d necesitamos realizar los pasos siguientes:

1. Recuperar mediante programación los datos que se va a enlazar a GridView (un `ProductsDataTable` instancia)
2. Ordenar los datos en función de las operaciones de asignación GridView `SortExpression` y `SortDirection` propiedades
3. Recorrer en iteración la `ProductsRows` en el `ProductsDataTable`, que buscan donde se encuentran las diferencias en la columna ordenada
4. En cada límite de grupo, insertar un registro de separador `ProductsRow` en la instancia a la tabla de datos, uno que tenga s `CategoryID` establecido en `-1` (o cualquier designación se elige para marcar un registro como registro separador)
5. Después de insertar las filas de separador, enlazar mediante programación los datos en GridView

Además de estos cinco pasos d también es necesario proporcionar un controlador de eventos para el s GridView `RowDataBound` eventos. En este caso, d comprobamos cada `DataRow` y determinar si es un separador de fila, uno cuyo `CategoryID` era `-1`. Si es así, d probablemente desee ajustar el texto mostrado en las celdas o su formato.

Con esta técnica para insertar el grupo de límites de ordenación que requiere un poco más trabajo que se ha descrito anteriormente, según sea necesario proporcionar también un controlador de eventos para el s GridView `Sorting` eventos y mantener un seguimiento de la `SortExpression` y `SortDirection` valores.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipular el s GridView controlar la recolección después s haya enlazado a datos

En lugar de los datos de mensajería antes de enlazar a GridView, podemos agregar las filas de separador *después* los datos se ha enlazado a la GridView. El proceso de enlace de datos se compila a la jerarquía de control GridView s, que en realidad es simplemente un `Table` instancia se compone de una colección de filas, cada uno de los cuales se compone de una colección de celdas. En concreto, la colección de controles de GridView s contiene un `Table` objeto en la raíz, un `GridViewRow` (que se deriva de la `TableRow` clase) para cada registro de la `DataSource` enlazar a GridView y un `TableCell` objeto en cada `GridViewRow` instancia para cada campo de datos en el `DataSource`.

Para agregar filas de separador entre cada grupo de ordenación, se puede manipular directamente esta jerarquía de control una vez que se ha creado. Podemos estar seguros de que se ha creado la jerarquía de control GridView s por última vez en el momento en que se está representando la página. Por lo tanto, este método invalida el `Page` clase s. `Render` método, momento en que la jerarquía de control final de GridView s se actualiza para incluir las filas de separador necesario. Figura 4 se ilustra este proceso.


[![Una técnica alternativa manipula la jerarquía de Control GridView s](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Figura 4**: una técnica alternativa manipula la jerarquía de Control de GridView s ([haga clic aquí para ver la imagen a tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image10.png))


Para este tutorial, usaremos este último enfoque para personalizar la experiencia del usuario de ordenación.

> [!NOTE]
> El código se m presentar en este tutorial se basa en el ejemplo proporcionado en [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) entrada del blog s [reproducir un Bit con agrupación de ordenación de GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Paso 3: Agregar las filas de separador en la jerarquía de Control GridView s

Puesto que deseamos solo agregar las filas de separador en la jerarquía de control GridView s después de su jerarquía de control se ha creado y creado por última vez en que visita de página, desea realizar esta adición al final del ciclo de vida de página, pero antes de la c de GridView real jerarquía de ontrol se ha representado en HTML. El último punto posible en el que podemos lograr esto es el `Page` clase s. `Render` evento, que es posible invalidar en nuestra clase de código subyacente mediante la firma de método siguiente:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Cuando el `Page` clase s original `Render` se invoca el método `base.Render(writer)` cada uno de los controles de la página se representará, generar el marcado en función de su jerarquía de control. Por lo tanto, es imperativo que ambos llamamos `base.Render(writer)`, de modo que se representa la página, y que se manipulan los s GridView control jerarquía antes de llamar a `base.Render(writer)`, de modo que las filas de separador se agregaron a la jerarquía de control GridView s antes de que s está representado.

Para insertar los encabezados de grupo de ordenación primero necesitamos asegurarnos de que el usuario ha solicitado que se ordenan los datos. De forma predeterminada, el contenido de s GridView no está ordenado y, por lo tanto, no queremos t necesario para escribir los encabezados de clasificación de grupos.

> [!NOTE]
> Si desea que el control GridView se ordenen por una columna en particular cuando se carga la página por primera vez, llamar a la GridView `Sort` método en la primera página, visite (pero no en postbacks posteriores). Para ello, agregue esta llamada en la `Page_Load` controlador de eventos dentro de un `if (!Page.IsPostBack)` condicional. Hacen referencia a la [paginación y ordenar datos de informe](paging-and-sorting-report-data-cs.md) información tutorial para obtener más información sobre la `Sort` método.


Suponiendo que los datos se han ordenado, la siguiente tarea consiste en determinar qué columna se ordenan los datos y, a continuación, para examinar las filas buscando las diferencias en esa columna s valores. El siguiente código garantiza que los datos se han ordenado y busca la columna por la que los datos se han ordenado:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Si tiene el control GridView aún esté ordenada, las operaciones de asignación GridView `SortExpression` propiedad no habrá se ha establecido. Por lo tanto, solamente queremos a agregar las filas de separador si esta propiedad tiene algún valor. Si es así, necesitamos a continuación determinar el índice de la columna por la que se ordenan los datos. Esto se logra mediante un bucle a través de las operaciones de asignación GridView `Columns` colección, búsqueda de la columna cuya `SortExpression` propiedad es igual a las operaciones de asignación GridView `SortExpression` propiedad. Además el índice de columna s, obtenemos también el `HeaderText` propiedad, que se utiliza para mostrar las filas de separador.

Con el índice de la columna por la que se ordenan los datos, el paso final es enumerar las filas del control GridView. Para cada fila es necesario determinar si el valor de la columna ordenada s difiere la anterior fila s ordenados s del valor de columna. Si por lo tanto, es necesario insertar un nuevo `GridViewRow` en la instancia a la jerarquía de control. Esto se consigue con el código siguiente:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Este código se inicia mediante programación que hacen referencia a la `Table` objeto encontrado en la raíz de la jerarquía de control GridView s y la creación de una variable de cadena denominada `lastValue`. `lastValue`se utiliza para comparar el valor de columna s ordenados de fila actual con el valor de fila s anterior. Siguiente, la s GridView `Rows` se enumera la colección y para cada fila se almacena el valor de la columna ordenada en la `currentValue` variable.

> [!NOTE]
> Para determinar el valor de la columna de s ordenados de fila determinado utilizar la celda s `Text` propiedad. Esto funciona bien para BoundFields, pero no se funcionan según sea necesario para TemplateFields, CheckBoxFields y así sucesivamente. Analizaremos cómo para tener en cuenta los campos de GridView alternativos en breve.


El `currentValue` y `lastValue` , a continuación, se comparan las variables. Si son diferentes que necesitamos agregar una nueva fila de separador en la jerarquía de control. Esto se logra mediante la determinación del índice de la `GridViewRow` en el `Table` objeto s `Rows` colección, crear nuevas `GridViewRow` y `TableCell` instancias y, a continuación, agregar el `TableCell` y `GridViewRow` a la jerarquía de control.

Tenga en cuenta que el separador de fila única s `TableCell` se da formato de manera que abarca todo el ancho del control GridView, se ha formateado con el `SortHeaderRowStyle` clase CSS y tiene su `Text` como muestra el grupo de ordenación nombre de propiedad (por ejemplo, la categoría) y el valor de s de grupo (por ejemplo, bebidas). Por último, `lastValue` se actualiza con el valor de `currentValue`.

La clase CSS que se usa para dar formato a la fila de encabezado de grupo ordenación `SortHeaderRowStyle` debe especificarse en el `Styles.css` archivo. No dude en usar la configuración de estilo atractivo; Utilizar lo siguiente:


[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Con el código actual, la interfaz de ordenación agrega encabezados de grupo de ordenación al ordenar por cualquier BoundField (Véase la figura 5, que muestra una captura de pantalla cuando se ordena por el proveedor). Sin embargo, al ordenar por cualquier otro tipo de campo (por ejemplo, un CampoCasillaVerificación o TemplateField), los encabezados de grupo de ordenación son ningún lugar donde se encuentra (vea la figura 6).


[![La interfaz de ordenación incluye encabezados de grupo de ordenación al ordenar por BoundFields](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Figura 5**: la ordenación interfaz incluye ordenación grupo encabezados cuando la ordenación por BoundFields ([haga clic aquí para ver la imagen a tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image13.png))


[![Los encabezados de grupo de ordenación son falta cuando un CampoCasillaVerificación la ordenación](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Figura 6**: los encabezados de grupo de ordenación son falta al ordenar un CampoCasillaVerificación ([haga clic aquí para ver la imagen a tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image16.png))


La razón que faltan los encabezados de grupo de ordenación al ordenar por un CampoCasillaVerificación es que el código usa actualmente solo el `TableCell` s `Text` propiedad para determinar el valor de la columna ordenada por cada fila. Para CheckBoxFields, el `TableCell` s `Text` propiedad es una cadena vacía; en su lugar, el valor está disponible a través de un control de casilla de verificación Web que se encuentra en la `TableCell` s `Controls` colección.

Para controlar los tipos de campo que no sea BoundFields, es necesario aumentar el código donde los `currentValue` variable se asigna a comprobar la existencia de un control CheckBox en el `TableCell` s `Controls` colección. En lugar de usar `currentValue = gvr.Cells[sortColumnIndex].Text`, reemplace este código con lo siguiente:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Este código examina la columna ordenada `TableCell` de la fila actual determinar si existen todos los controles en el `Controls` colección. Si hay, y el primer control es una casilla, la `currentValue` variable se establece en Sí o No, dependiendo de la casilla de verificación s `Checked` propiedad. En caso contrario, el valor se toma de la `TableCell` s `Text` propiedad. Esta lógica se puede replicar para controlar la ordenación para cualquier TemplateFields que existan en GridView.

Con la adición de código anterior, ahora están presentes cuando se ordena por el CampoCasillaVerificación no incluye los encabezados de grupo de ordenación (consulte la figura 7).


[![Los encabezados de grupo de ordenación son ahora presente al ordenar un CampoCasillaVerificación](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Figura 7**: los encabezados de grupo de ordenación son ahora presente al ordenar un CampoCasillaVerificación ([haga clic aquí para ver la imagen a tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image19.png))


> [!NOTE]
> Si tiene productos con `NULL` valores para la base de datos la `CategoryID`, `SupplierID`, o `UnitPrice` campos, esos valores se mostrarán como cadenas vacías en el control GridView de forma predeterminada, lo que significa que el texto de fila s separador para esos productos con `NULL`valores aparecerán como categoría: (es decir, hay s ningún nombre después de la categoría: al igual que con la categoría: bebidas). Si desea que un valor que se muestra aquí se puede establecer la BoundFields [ `NullDisplayText` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) al texto que desea mostrar o puede agregar una instrucción condicional en el método Render al asignar la `currentValue` para el separador fila s `Text` propiedad.


## <a name="summary"></a>Resumen

GridView no incluye muchas opciones integradas para personalizar la interfaz de ordenación. Sin embargo, con un poco de código de bajo nivel, s posible retocar la jerarquía de control GridView s para crear una interfaz más personalizada. En este tutorial, hemos visto cómo agregar una fila de separador de grupo de ordenación para un control GridView que se puede ordenar, que permite identificar más fácilmente los distintos grupos y los límites de grupos. Para obtener ejemplos adicionales de interfaces de ordenación personalizadas, visite [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [unos ASP.NET 2.0 GridView ordenación sugerencias y trucos](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) entrada de blog.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](sorting-custom-paged-data-cs.md)
[Siguiente](paging-and-sorting-report-data-vb.md)
