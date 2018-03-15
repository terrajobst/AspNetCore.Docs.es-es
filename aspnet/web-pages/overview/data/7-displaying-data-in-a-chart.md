---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: "Mostrar datos en un gráfico con ASP.NET Web Pages (Razor) | Documentos de Microsoft"
author: microsoft
description: "Este capítulo explica cómo mostrar datos en un gráfico. En los capítulos anteriores, aprendió a mostrar los datos manualmente y en una cuadrícula. Este capítulo explica..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: f252b74bc42d0ea65b8b1150973c4f3c50cc9cf4
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Mostrar datos en un gráfico con las páginas Web ASP.NET (Razor)
====================
por [Microsoft](https://github.com/microsoft)

> Este artículo explica cómo usar un gráfico para mostrar datos en un sitio Web de ASP.NET Web Pages (Razor), use el `Chart` auxiliar.
> 
> **Lo que aprenderá**:
> 
> - Cómo mostrar los datos en un gráfico.
> - Cómo crear gráficos con temas integrados.
> - Cómo guardar gráficos y cómo almacenarlas en caché para mejorar el rendimiento.
> 
> Se trata de la programación de características introducidas en el artículo de ASP.NET:
> 
> - El `Chart` auxiliar.
> 
> > [!NOTE]
> > La información de este artículo se aplica a páginas Web de ASP.NET 1.0 y 2 páginas Web.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>La aplicación auxiliar de gráfico

Cuando desea mostrar los datos en forma gráfica, puede usar `Chart` auxiliar. El `Chart` auxiliar puede presentar una imagen que muestra datos en una variedad de tipos de gráficos. Admite muchas opciones para aplicar formato y el etiquetado. El `Chart` auxiliar puede procesar más de 30 tipos de gráficos, incluidos todos los tipos de gráficos que es posible que esté familiarizado con desde Microsoft Excel u otras herramientas &#8212; gráficos de áreas, gráficos de barras, gráficos de columnas, gráficos de líneas y los gráficos circulares, junto con más gráficos especializados, como los gráficos de cotizaciones.

| **Gráfico de áreas** ![Descripción: imagen del tipo de gráfico de área](7-displaying-data-in-a-chart/_static/image1.jpg) | **Gráfico de barras** ![Descripción: imagen del tipo de gráfico de barras](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Gráfico de columnas** ![Descripción: imagen del tipo de gráfico de columna](7-displaying-data-in-a-chart/_static/image3.jpg) | **Gráfico de líneas** ![Descripción: imagen del tipo de gráfico de línea](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Gráfico circular** ![Descripción: imagen del tipo de gráfico circular](7-displaying-data-in-a-chart/_static/image5.jpg) | **Gráfico de cotizaciones** ![Descripción: imagen del tipo de gráfico de cotizaciones](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elemento de gráfico

Los gráficos muestran datos y elementos adicionales, como leyendas, ejes, series y así sucesivamente. La siguiente imagen muestra muchos de los elementos de gráfico que se pueden personalizar cuando se usa el `Chart` auxiliar. Este artículo muestra cómo establecer algunos (no todos) de estos elementos.

![Descripción: Imagen los elementos del gráfico](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Crear un gráfico de datos

Los datos que muestren en un gráfico pueden ser de una matriz de los resultados devueltos de una base de datos o de los datos que se están en un archivo XML.

### <a name="using-an-array"></a>Usar una matriz

Como se explica en [Introducción a ASP.NET Web Pages de programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890), una matriz le permite almacenar una colección de elementos similares en una única variable. Puede utilizar matrices para que contenga los datos que se van a incluir en el gráfico.

Este procedimiento muestra cómo crear un gráfico de datos en las matrices, mediante el tipo de gráfico predeterminado. También muestra cómo mostrar el gráfico dentro de la página.

1. Crear un nuevo archivo denominado *ChartArrayBasic.cshtml*.
2. Reemplace el contenido existente con lo siguiente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    En primer lugar, el código crea un nuevo gráfico y establece su ancho y alto. Especificar el título del gráfico mediante la `AddTitle` método. Para agregar datos, use la `AddSeries` método. En este ejemplo, se usa el `name`, `xValue`, y `yValues` parámetros de la `AddSeries` método. El `name` parámetro se muestra en la leyenda del gráfico. El `xValue` parámetro contiene una matriz de datos que se muestran en el eje horizontal del gráfico. El `yValues` parámetro contiene una matriz de datos que se usan para trazar los puntos verticales del gráfico.

    El `Write` método representa realmente el gráfico. En este caso, ya que no ha especificado un tipo de gráfico, el `Chart` aplicación auxiliar representa su gráfico predeterminado, que es un gráfico de columnas.
3. Ejecute la página en el explorador. El explorador muestra el gráfico. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Utilizar una consulta de base de datos para los datos de gráfico

Si la información que desee incluir en el gráfico está en una base de datos, puede ejecutar una consulta de base de datos y, a continuación, utilizar datos de los resultados para crear el gráfico. Este procedimiento muestra cómo leer y mostrar los datos de la base de datos que creó en el artículo [Introducción a trabajar con una base de datos en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Agregar un *aplicación\_datos* carpeta a la raíz del sitio Web si la carpeta no existe.
2. En el *aplicación\_datos* carpeta, agregue el archivo de base de datos denominado *SmallBakery.sdf* que se describe en [Introducción a trabajar con una base de datos en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Crear un nuevo archivo denominado *ChartDataQuery.cshtml*.
4. Reemplace el contenido existente con lo siguiente:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    El código primero abre la base de datos de SmallBakery y lo asigna a una variable denominada `db`. Esta variable representa un `Database` objeto que puede utilizarse para leer y escribir en la base de datos. A continuación, el código ejecuta una consulta SQL para obtener el nombre y el precio de cada producto. El código crea un nuevo gráfico y le pasa la consulta de base de datos mediante una llamada a del gráfico `DataBindTable` método. Este método toma dos parámetros: el `dataSource` parámetro es para los datos de la consulta y el `xField` parámetro le permite establecer la columna de datos se usa para el eje x del gráfico.

    Como alternativa al uso de la `DataBindTable` método, puede usar el `AddSeries` método de la `Chart` auxiliar. El `AddSeries` método le permite definir la `xValue` y `yValues` parámetros. Por ejemplo, en lugar de utilizar el `DataBindTable` método similar al siguiente:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Puede usar el `AddSeries` método similar al siguiente:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Ambas representan los mismos resultados. El `AddSeries` método es más flexible porque se pueden especificar el tipo de gráfico y los datos más explícitamente, pero la `DataBindTable` método es más fácil usar si no necesita la flexibilidad adicional.
5. Ejecute la página en un explorador. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Usar datos XML

La tercera opción de gráficos es usar un archivo XML como los datos para el gráfico. Esto requiere que el archivo XML tiene también un archivo de esquema (*.xsd* archivo) que describe la estructura XML. Este procedimiento muestra cómo leer datos desde un archivo XML.

1. En el *aplicación\_datos* carpeta, cree un nuevo archivo XML denominado *data.xml*.
2. Reemplace el XML existente con los siguientes valores, que es algunos datos XML acerca de los empleados de una compañía ficticia. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. En el *aplicación\_datos* carpeta, cree un nuevo archivo XML denominado *data.xsd*. (Tenga en cuenta que la extensión de este tiempo es *.xsd*.)
4. Reemplace el XML existente con lo siguiente: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. En la raíz del sitio Web, cree un nuevo archivo denominado *ChartDataXML.cshtml*.
6. Reemplace el contenido existente con lo siguiente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    El código crea primero un `DataSet` objeto. Este objeto se usa para administrar los datos que se leen desde el archivo XML y organizan según la información en el archivo de esquema. (Observe que la parte superior del código incluye la instrucción `using SystemData`. Esto es necesario para poder trabajar con el `DataSet` objeto. Para obtener más información, consulte [ &quot;mediante&quot; instrucciones y nombres completos](#SB_UsingStatements) más adelante en este artículo.)

    A continuación, el código crea un `DataView` objeto basado en el conjunto de datos. La vista de datos proporciona un objeto que se puede enlazar el gráfico &#8212; es decir, lectura y trazar. El gráfico se enlaza a los datos mediante la `AddSeries` método, como se vio anteriormente al representar gráficamente los datos de matriz, salvo que esta vez el `xValue` y `yValues` parámetros están establecidos en la `DataView` objeto.

    En este ejemplo también muestra cómo especificar un tipo de gráfico determinado. Cuando los datos se agregan en el `AddSeries` método, el `chartType` parámetro también se establece para mostrar un gráfico circular.
7. Ejecute la página en un explorador. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP] 
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Instrucciones "Using" y nombres completos
> 
> .NET Framework que ASP.NET Web Pages con sintaxis Razor se basa en consta de varios miles de componentes (clases). Para que resulte sencillo trabajar con todas estas clases, organizan en *espacios de nombres*, que son similares a las bibliotecas. Por ejemplo, el `System.Web` espacio de nombres contiene clases que admiten la comunicación de explorador y el servidor, el `System.Xml` espacio de nombres contiene clases que se usan para crear y leer archivos XML, y el `System.Data` espacio de nombres contiene clases que permiten trabajar con los datos.
> 
> Para tener acceso a cualquier clase determinada de .NET Framework, necesita conocer no sólo el nombre de clase, sino también el espacio de nombres que la clase está en código. Por ejemplo, para poder usar el `Chart` aplicación auxiliar, el código debe buscar la `System.Web.Helpers.Chart` (clase), que combina el espacio de nombres (`System.Web.Helpers`) con el nombre de clase (`Chart`). Esto se conoce como la clase *completo* nombre &#8212; su ubicación completa y no ambigua en la gran amplitud de .NET Framework. En el código, esto sería similar al siguiente:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Sin embargo, resulta complicado realizar cualquier operación (y es proclive a errores) para utilizar estos nombres largos y completo cada vez que desee hacer referencia a una clase o una aplicación auxiliar. Por lo tanto, para que sea más fácil de usar nombres de clase, puede *importar* los espacios de nombres que le interesa, que suele ser es simplemente un puñado entre muchos espacios de nombres en .NET Framework. Si ha importado un espacio de nombres, puede usar un nombre de clase (`Chart`) en lugar del nombre completo (`System.Web.Helpers.Chart`). Cuando el código se ejecuta y encuentra un nombre de clase, puede buscar solo los espacios de nombres que ha importado para encontrar esa clase.
> 
> Cuando usa ASP.NET Web Pages con sintaxis Razor para crear páginas web, normalmente utiliza el mismo conjunto de clases cada vez, incluida la `WebPage` clase, las aplicaciones auxiliares diversos y así sucesivamente. Para guardar el trabajo de importación de los espacios de nombres pertinentes cada vez que cree un sitio Web, ASP.NET se configura para que importa automáticamente un conjunto de espacios de nombres básicos para todos los sitios Web. Por eso no he tenido que tratar con espacios de nombres o importar hasta ahora; todas las clases que ha trabajado con están en espacios de nombres que se importan automáticamente.
> 
> Sin embargo, en ciertas ocasiones necesitará trabajar con una clase que no se encuentra en un espacio de nombres que se importará automáticamente. En ese caso, puede usar el nombre de acceso completa de la clase, o puede importar manualmente el espacio de nombres que contiene la clase. Para importar un espacio de nombres, use el `using` instrucción (`import` en Visual Basic), tal y como se vio en un ejemplo anterior el artículo.
> 
> Por ejemplo, el `DataSet` clase está en el `System.Data` espacio de nombres. El `System.Data` espacio de nombres no está disponible automáticamente para las páginas de ASP.NET Razor. Por lo tanto, para trabajar con la `DataSet` clase utilizando su nombre completo, puede utilizar código similar al siguiente:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Si tiene que usar el `DataSet` clase varias veces puede importar un espacio de nombres similar al siguiente y, a continuación, use simplemente el nombre de clase en el código:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Puede agregar `using` instrucciones para otros espacios de .NET Framework que desea hacer referencia a. Sin embargo, como se indicó, no necesita hacer esto a menudo, dado que la mayoría de las clases que trabajará con está en espacios de nombres que se importan automáticamente por ASP.NET para su uso en *.cshtml* y *.vbhtml* páginas.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Visualización de gráficos dentro de una página Web

En los ejemplos se ha visto hasta ahora, se crea un gráfico y, a continuación, el gráfico se representa directamente en el explorador como un gráfico. En muchos casos, sin embargo, desea mostrar un gráfico como parte de una página, no solo por sí mismo en el explorador. Para ello requiere un proceso de dos pasos. El primer paso es crear una página que genera el gráfico, tal y como ya ha visto.

El segundo paso es mostrar la imagen resultante en otra página. Para mostrar la imagen, debe usar un elemento HTML `<img>` elemento, en la misma manera que lo haría para mostrar cualquier imagen. Sin embargo, en lugar de hacer referencia a un *.jpg* o *.png* archivo, el `<img>` referencias del elemento de la *.cshtml* archivo que contiene el `Chart` auxiliar que crea el gráfico. Cuando se ejecuta la página de presentación, el `<img>` elemento recibe el resultado de la `Chart` auxiliar y representa el gráfico.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Cree un archivo denominado *ShowChart.cshtml*.
2. Reemplace el contenido existente con lo siguiente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    El código usa el `<img>` elemento que se va a mostrar el gráfico que creó anteriormente en el *ChartArrayBasic.cshtml* archivo.
3. Ejecute la página web en un explorador. El *ShowChart.cshtml* archivo muestra la imagen de gráfico basándose en el código incluido en el *ChartArrayBasic.cshtml* archivo.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Aplicar un estilo a un gráfico

El `Chart` auxiliar es compatible con un gran número de opciones que le permiten personalizar la apariencia del gráfico. Puede establecer colores, fuentes, bordes y así sucesivamente. Una manera fácil de personalizar la apariencia de un gráfico es usar un *tema*. Los temas son colecciones de información que especifican cómo presentar un gráfico utilizando fuentes, colores, etiquetas, paletas, bordes y efectos. (Tenga en cuenta que el estilo de un gráfico no indica el tipo de gráfico).

En la tabla siguiente se enumera temas integrados.

| Tema | Descripción |
| --- | --- |
| `Vanilla` | Muestra columnas rojas sobre un fondo blanco. |
| `Blue` | Muestra las columnas de un fondo degradado azul en azul. |
| `Green` | Muestra las columnas de un fondo degradado verde en azul. |
| `Yellow` | Muestra columnas naranjas sobre un fondo amarillo degradado. |
| `Vanilla3D` | Muestra las columnas 3D de color rojo sobre un fondo blanco. |

Puede especificar el tema que se usará al crear un nuevo gráfico.

1. Crear un nuevo archivo denominado *ChartStyleGreen.cshtml*.
2. Reemplace el contenido existente en la página con lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Este código es el mismo que el ejemplo anterior que usa la base de datos para los datos, pero agrega la `theme` parámetro cuando crea el `Chart` objeto. A continuación muestra el código modificado:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Ejecute la página en un explorador. Vea los mismos datos que antes, pero el gráfico parece más refinado: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Guardar un gráfico

Cuando se usa el `Chart` auxiliar como se ha visto hasta ahora en este artículo, la aplicación auxiliar vuelve a crea el gráfico desde el principio cada vez que se invoca. Si es necesario, el código para el gráfico también vuelve a consulta la base de datos o vuelve a lee el archivo XML para obtener los datos. En algunos casos, esto puede ser una operación compleja, como si la base de datos que está consultando es grande, o si el archivo XML contiene una gran cantidad de datos. Incluso si el gráfico no implica una gran cantidad de datos, el proceso de creación dinámica de una imagen ocupa recursos del servidor y, si muchas personas solicitan la página o páginas que se muestran en el gráfico, puede haber un impacto en el rendimiento de su sitio Web.

Para ayudar a reducir el impacto del rendimiento de creación de un gráfico, puede crear una gráfico la primera vez que lo necesite y, a continuación, guárdelo. Cuando el gráfico se vuelva a necesitar, en lugar de volver a generarlo, puede capturar la versión guardada y presentar eso.

Puede guardar un gráfico de las siguientes maneras:

- Almacenar en caché el gráfico en la memoria del equipo (en el servidor).
- Guarde el gráfico como un archivo de imagen.
- Guarde el gráfico como un archivo XML. Esta opción le permite modificar el gráfico antes de guardarla.

### <a name="caching-a-chart"></a>Almacenamiento en caché de un gráfico

Después de crear un gráfico, puede almacenar en caché se. Almacenamiento en caché de un gráfico significa que no tiene que volver a crearse si necesita que se mostrará de nuevo. Cuando se guarda un gráfico en la memoria caché, se proporciona una clave que debe ser única para ese gráfico.

Los gráficos guardados en la memoria caché podrían quitarse si el servidor se queda sin memoria. Además, la memoria caché se borra si se reinicia la aplicación por algún motivo. Por lo tanto, la manera estándar para trabajar con un gráfico en caché es comprobar siempre en primer lugar, si está disponible en la memoria caché y, si no es así, a continuación, crear o volver a crearla.

1. En la raíz del sitio Web, cree un archivo denominado *ShowCachedChart.cshtml*.
2. Reemplace el contenido existente con lo siguiente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    El `<img>` etiqueta incluye un `src` atributo que apunta a la *ChartSaveToCache.cshtml* archivo y pasa una clave a la página como una cadena de consulta. La clave contiene el valor &quot;myChartKey&quot;. El *ChartSaveToCache.cshtml* archivo contiene el `Chart` auxiliar que crea el gráfico. Esta página se creará en un momento.

    Al final de la página, hay un vínculo a una página denominada *ClearCache.cshtml*. Que es una página que va a crear en breve. Necesita el *ClearCache.cshtml* sólo para probar el almacenamiento en caché para este ejemplo, no es un vínculo o una página que normalmente incluiría al trabajar con gráficos almacenados en caché.
3. En la raíz del sitio Web, cree un nuevo archivo denominado *ChartSaveToCache.cshtml*.
4. Reemplace el contenido existente con lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    El código comprueba primero si nada se pasó como el valor de clave en la cadena de consulta. Si es así, el código intenta leer un gráfico de la memoria caché mediante una llamada a la `GetFromCache` método y pasar la clave. Si parece que no hay nada en la memoria caché bajo esa clave (que se realizarían la primera vez que se solicita el gráfico), el código crea el gráfico como de costumbre. Cuando haya finalizado el gráfico, el código lo guarda en la memoria caché mediante una llamada a `SaveToCache`. Este método requiere una clave (de manera que el gráfico se puede solicitar más adelante) y la cantidad de tiempo que el gráfico debe guardarse en la memoria caché. (El tiempo exacto que se podría almacenar en caché un gráfico dependería de la frecuencia con considera pueden cambiar los datos que representa.) El `SaveToCache` método también requiere una `slidingExpiration` parámetro &#8212; si se establece en true, el tiempo de espera de contador se restablece cada vez que se tiene acceso el gráfico. En este caso, en realidad significa que la entrada de caché del gráfico expira 2 minutos desde la última vez que alguien tiene acceso el gráfico. (La alternativa para la expiración variable está expiración absoluta, lo que significa que la entrada de caché caducará exactamente dos minutos después de que se ha puesto en la memoria caché, con independencia de la frecuencia con había accedido).

    Por último, el código usa el `WriteFromCache` método para capturar y representar el gráfico de la memoria caché. Tenga en cuenta que este método está fuera del `if` bloque que comprueba la memoria caché, ya que obtendrá el gráfico de la memoria caché si el gráfico estaba presente con el que comenzar o tenía que se genera y se guarda en la memoria caché.

    Tenga en cuenta que en el ejemplo, el `AddTitle` método incluye una marca de tiempo. (Agregan la fecha y hora actuales &#8212; `DateTime.Now` &#8212; al título.)
5. Crear una nueva página denominada *ClearCache.cshtml* y reemplace su contenido por lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Esta página usa la `WebCache` auxiliar para quitar el gráfico que se almacena en caché *ChartSaveToCache.cshtml*. Tal y como se indicó anteriormente, normalmente no deben tener una página similar al siguiente. Va a crear aquí sólo para que resulten más fáciles de probar el almacenamiento en caché.
6. Ejecute el *ShowCachedChart.cshtml* página web en un explorador. La página muestra la imagen de gráfico basándose en el código incluido en el *ChartSaveToCache.cshtml* archivo. Tome nota de qué dice la marca de tiempo en el título del gráfico. 

    ![Descripción: Imagen de gráfico básico con marca de tiempo en el título del gráfico](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Cierre el explorador.
8. Ejecute el *ShowCachedChart.cshtml* nuevo. Observe que la marca de tiempo es el mismo que antes, lo que indica que el gráfico no se vuelven a generar, pero en su lugar se leyó desde la memoria caché.
9. En *ShowCachedChart.cshtml*, haga clic en el **Borrar caché** vínculo. Esto le llevará al *ClearCache.cshtml*, que informa de que se ha borrado la memoria caché.
10. Haga clic en el **volver a ShowCachedChart.cshtml** vincular o vuelva a ejecutar *ShowCachedChart.cshtml* desde WebMatrix. Tenga en cuenta que ha cambiado este momento la marca de tiempo, porque se ha borrado la memoria caché. Por lo tanto, el código se tenía que volver a generar el gráfico y se vuelven a poner en la memoria caché.

### <a name="saving-a-chart-as-an-image-file"></a>Guardar un gráfico como un archivo de imagen

También puede guardar un gráfico como un archivo de imagen (por ejemplo, como un *.jpg* archivo) en el servidor. A continuación, puede usar el archivo de imagen tal como lo haría con cualquier imagen. La ventaja es que el archivo se almacena en lugar de guardar en una caché temporal. Puede guardar una nueva imagen de gráfico en momentos diferentes (por ejemplo, cada hora) y, a continuación, mantener un registro permanente de los cambios que se producen con el tiempo. Tenga en cuenta que debe asegurarse de que la aplicación web tiene permiso para guardar un archivo en la carpeta en el servidor donde desea colocar el archivo de imagen.

1. En la raíz del sitio Web, cree una carpeta denominada  *\_ChartFiles* si aún no existe.
2. En la raíz del sitio Web, cree un nuevo archivo denominado *ChartSave.cshtml*.
3. Reemplace el contenido existente con lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    El código comprueba primero si el *.jpg* archivo existe mediante una llamada a la `File.Exists` método. Si el archivo no existe, el código crea un nuevo `Chart` desde una matriz. Esta vez, el código llama el `Save` método y pasa el `path` para especificar la ruta de acceso y nombre de archivo donde se guardará el gráfico. En el cuerpo de la página, un `<img>` elemento usa la ruta de acceso para que apunte a la *.jpg* archivo para mostrar.
4. Ejecute el *ChartSave.cshtml* archivo.
5. Volver a WebMatrix. Tenga en cuenta que un archivo de imagen denominado *chart01.jpg* se ha guardado en el  *\_ChartFiles* carpeta.

### <a name="saving-a-chart-as-an-xml-file"></a>Guardar un gráfico como un archivo XML

Por último, puede guardar un gráfico como un archivo XML en el servidor. Una ventaja de usar este método en el gráfico en caché o guardar el gráfico en un archivo es que puede modificar el XML antes de mostrar el gráfico si deseara. La aplicación debe tener permisos de lectura/escritura para la carpeta en el servidor donde desea colocar el archivo de imagen.

1. En la raíz del sitio Web, cree un nuevo archivo denominado *ChartSaveXml.cshtml*.
2. Reemplace el contenido existente con lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Este código es similar al código que vio anteriormente para almacenar un gráfico en la memoria caché, salvo que usa un archivo XML. El código comprueba primero si existe el archivo XML mediante una llamada a la `File.Exists` método. Si el archivo existe, el código crea un nuevo `Chart` objeto y pasa el nombre de archivo como la `themePath` parámetro. Esto crea el gráfico basándose en lo que se encuentra en el archivo XML. Si aún no existe el archivo XML, el código crea un gráfico como lo haría normalmente y, a continuación, llama a `SaveXml` para guardarlo. El gráfico se representa mediante el `Write` método, como se ha visto antes.

    Al igual que con la página que el almacenamiento en caché se ha explicado, este código incluye una marca de tiempo en el título del gráfico.
3. Crear una nueva página denominada *ChartDisplayXMLChart.cshtml* y agregue el siguiente marcado al mismo: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Ejecute el *ChartDisplayXMLChart.cshtml* página. Se muestra el gráfico. Tome nota de la marca de tiempo en el título del gráfico.
5. Cierre el explorador.
6. En WebMatrix, haga clic en el  *\_ChartFiles* carpeta, haga clic en **actualizar**y, a continuación, abra la carpeta. El *XMLChart.xml* archivo en esta carpeta se creó con la `Chart` auxiliar. 

    ![Descripción: _ChartFiles carpeta que muestra el archivo XMLChart.xml creado por la aplicación auxiliar de gráfico.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Ejecute el *ChartDisplayXMLChart.cshtml* página de nuevo. El gráfico muestra la misma marca de tiempo como la primera vez que ejecutó la página. Eso es porque se está generando el gráfico del archivo XML que guardó anteriormente.
8. En WebMatrix, abra el  *\_ChartFiles* carpeta y eliminar la *XMLChart.xml* archivo.
9. Ejecute el *ChartDisplayXMLChart.cshtml* página una vez más. Esta vez, se actualiza la marca de tiempo, porque el `Chart` auxiliar tenía que volver a crear el archivo XML. Si lo desea, compruebe la  *\_ChartFiles* carpeta y observe que el archivo XML es nuevo.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a trabajar con una base de datos en ASP.NET Web Pages sitios](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Usar almacenamiento en caché en ASP.NET Web Pages sitios para mejorar el rendimiento](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Clase de gráfico](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (referencia de la API de páginas Web de ASP.NET en MSDN)
