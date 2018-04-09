---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interactuar con la página principal de la página de contenido (VB) | Documentos de Microsoft
author: rick-anderson
description: Examina cómo llamar a métodos, establecer propiedades, etc. de la página maestra desde el código en la página de contenido.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 7e92e7abbc69a0998d7563db0d5b206d7ba3d6be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interactuar con la página principal de la página de contenido (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Examina cómo llamar a métodos, establecer propiedades, etc. de la página maestra desde el código en la página de contenido.


## <a name="introduction"></a>Introducción

En el transcurso de los últimos cinco tutoriales que hemos examinado cómo crear una página maestra, definir áreas de contenido, enlazar las páginas ASP.NET a una página maestra y definen el contenido específico de la página. Cuando un visitante solicita una página de contenido determinada, el contenido y el marcado de páginas maestras se fusionan en tiempo de ejecución, lo que produce la representación de una jerarquía de un control unificado. Por lo tanto, ya hemos visto una manera en que pueden interactuar a la página maestra y una de las páginas de contenido: la página de contenido se explica los el marcado para transfuse en controles ContentPlaceHolder de la página maestra.

¿Qué tenemos todavía para examinar es cómo pueden interactuar mediante programación la página maestra y la página de contenido. Además de definir el marcado para los controles de la página maestra ContentPlaceHolder, una página de contenido también puede asignar valores a las propiedades públicas de su página maestra e invocar sus métodos públicos. De forma similar, una página maestra puede interactuar con las páginas de contenido. Aunque interacción mediante programación entre una página maestra y el contenido es menos común que la interacción entre su marcado declarativo, hay muchos escenarios donde es necesario este tipo interacción mediante programación.

En este tutorial se examina cómo una página de contenido mediante programación puede interactuar con su página principal; en el siguiente tutorial, veremos cómo la página principal del mismo modo puede interactuar con las páginas de contenido.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Ejemplos de programación interacción entre una página de contenido y su página principal

Cuando una región determinada de una página debe configurarse según una página a página, usamos un control ContentPlaceHolder. Pero ¿qué hay de situaciones donde la mayoría de las páginas se necesita para emitir un resultado determinado, pero un pequeño número de páginas necesitan personalizarlo para mostrar algo más? Un ejemplo de este tipo, que se examina en el [ *varios ContentPlaceHolders y contenido predeterminado* ](multiple-contentplaceholders-and-default-content-vb.md) implica el tutorial, mostrar una interfaz de inicio de sesión en cada página. Aunque la mayoría de las páginas debe incluir una interfaz de inicio de sesión, se debe suprimir para una serie de páginas, como: la página de inicio de sesión principal (`Login.aspx`); la página Crear cuenta; y las demás páginas que solo están disponibles para los usuarios autenticados. El [ *varios ContentPlaceHolders y contenido predeterminado* ](multiple-contentplaceholders-and-default-content-vb.md) tutorial se ha explicado cómo definir el contenido predeterminado para un control ContentPlaceHolder en la página maestra y, a continuación, cómo invalidar en las páginas donde el no se desea el contenido predeterminado.

Otra opción es crear una propiedad pública o un método en la página maestra que indica si se debe mostrar u ocultar la interfaz de inicio de sesión. Por ejemplo, la página maestra puede incluir una propiedad pública denominada `ShowLoginUI` cuyo valor se utiliza para establecer el `Visible` propiedad del control de inicio de sesión en la página maestra. Esas páginas de contenido que se debe suprimir la interfaz de usuario de inicio de sesión, a continuación, pueden establecer mediante programación el `ShowLoginUI` propiedad `False`.

Quizás el ejemplo más común de contenido e interacción de página maestra se produce cuando los datos se muestran en la página maestra debe actualizarse después de que haya llegado alguna acción en la página de contenido. Considere la posibilidad de una página maestra que incluya un control GridView que muestra los cinco más recientemente agregaron registros de una tabla de base de datos determinada y que uno de sus páginas de contenido incluye una interfaz para agregar nuevos registros a la misma tabla.

Cuando un usuario visita la página para agregar un nuevo registro, ve que las cinco agregan más recientemente registros que se muestran en la página maestra. Después de rellenar los valores de columnas del nuevo registro, envía el formulario. Suponiendo que el control GridView en la página maestra tiene su `EnableViewState` propiedad establecida en True (valor predeterminado), se vuelve a cargar su contenido del estado de vista y, por lo tanto, se muestran los mismos cinco registros, aunque un registro más reciente se acaba de agregar a la base de datos. Esto puede confundir al usuario.

> [!NOTE]
> Incluso si se deshabilita el estado de vista de GridView para que vuelve a enlazar a su origen de datos subyacente en cada postback, todavía no aparece el registro recién agregado porque los datos se enlazan a la GridView anteriormente en el ciclo de vida de la página que cuando se agrega el nuevo registro a la datab ase.


Para solucionar este problema para que el registro recién agregada se muestra en la página principal del GridView en devolución de datos es necesario para indicar a la GridView a enlazarse a su origen de datos *después* se ha agregado el nuevo registro a la base de datos. Esto requiere la interacción entre el contenido y páginas maestras porque es la interfaz para agregar que el nuevo registro (y sus controladores de eventos) se encuentran en la página de contenido, pero el control GridView que debe actualizarse en la página maestra.

Dado que actualizar la presentación de la página principal de un controlador de eventos en la página de contenido es una de las necesidades más comunes para el contenido y la interacción de página maestra, vamos a examinar en este tema con más detalle. La descarga de este tutorial incluye una base de datos de Microsoft SQL Server 2005 Express Edition con nombre `NORTHWIND.MDF` en el sitio de Web `App_Data` carpeta. La base de datos de Northwind almacena información de ventas de una compañía ficticia, Northwind Traders, empleados y producto.

Paso 1 recorre mostrar las cinco más recientemente habían agregado productos en un control GridView en la página maestra. Paso 2 crea una página de contenido para agregar nuevos productos. Paso 3 examina cómo crear propiedades y métodos públicos en la página maestra y paso 4 se muestra cómo establecer una conexión mediante programación con estas propiedades y métodos de la página de contenido.

> [!NOTE]
> Este tutorial no profundizar en los detalles sobre cómo trabajar con datos en ASP.NET. Los pasos de configuración de la página maestra para mostrar los datos y la página de contenido para insertar datos son completa, todavía viento suave. Para obtener información más detallada en Mostrar y la inserción de datos y cómo usar los controles SqlDataSource y GridView, consulte los recursos en la sección Lecturas adicionales al final de este tutorial.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Paso 1: Mostrar los cinco más recientemente agregado productos en la página maestra

Abra la página maestra Site.master y agregue una etiqueta y un control GridView para la `leftContent` `<div>`. Desactive fuera de la etiqueta `Text` establecer la propiedad, su `EnableViewState` propiedad `False`y su `ID` propiedad `GridMessage`; establecer la GridView `ID` propiedad a `RecentProducts`. A continuación, en el diseñador, expanda la etiqueta inteligente de GridView y elija enlazar a un nuevo origen de datos. Esto inicia al Asistente para configuración de origen de datos. Dado que la base de datos Northwind en el `App_Data` carpeta es una base de datos de Microsoft SQL Server, decide crear un SqlDataSource seleccionando (vea la figura 1); nombre SqlDataSource `RecentProductsDataSource`.


[![Enlazar el control GridView a un Control SqlDataSource denominado RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Figura 01**: enlazar el control GridView a un Control SqlDataSource denominado `RecentProductsDataSource` ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


El siguiente paso le pide que especifique qué base de datos para conectarse a. Elija la `NORTHWIND.MDF` base de datos de archivo en la lista desplegable y haga clic en siguiente. Dado que esta es la primera vez que hemos usado esta base de datos, el asistente ofrecerá almacenar la cadena de conexión en `Web.config`. Tiene almacenar la cadena de conexión con el nombre `NorthwindConnectionString`.


[![Conectarse a la base de datos Northwind](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Figura 02**: conectarse a la base de datos Northwind ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


El Asistente para configurar orígenes de datos proporciona por el que se puede especificar la consulta utilizada para recuperar datos de dos formas:

- Mediante la especificación de una instrucción SQL personalizada o un procedimiento almacenado, o
- Seleccionar una tabla o vista y, a continuación, especificando las columnas que se va a devolver

Puesto que deseamos devolver que solo los cinco productos haya agregado más recientemente, es preciso especificar una instrucción SQL personalizada. Utilice la siguiente `SELECT` consulta:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

El `TOP 5` palabra clave devuelve únicamente los cinco primeros registros de la consulta. El `Products` clave principal de la tabla, `ProductID`, es un `IDENTITY` columna, lo que nos garantiza que cada nuevo producto que se agrega a la tabla tendrá un valor mayor que la entrada anterior. Por lo tanto, ordenar los resultados por `ProductID` en orden descendente devuelve los productos a partir de la última creadas nuevamente.


[![Devolver los cinco productos agregados más recientemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Figura 03**: devolver los cinco más recientemente agregado productos ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


Después de completar el asistente, Visual Studio genera dos BoundFields del control GridView mostrar el `ProductName` y `UnitPrice` campos se devuelven desde la base de datos. En este momento marcado declarativo de la página maestra debe incluir marcado similar al siguiente:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Como puede apreciar, contiene el marcado: el control Web Label (`GridMessage`); GridView `RecentProducts`, con dos BoundFields; y un control SqlDataSource que devuelve los cinco productos haya agregado más recientemente.

Con esto GridView que creó y su control SqlDataSource configurado, visite el sitio Web a través de un explorador. Como se muestra en la figura 4, verá una cuadrícula en la esquina inferior izquierda que muestra los cinco más recientemente agregado productos.


[![GridView muestra los cinco productos agregados más recientemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Figura 04**: GridView muestra los cinco más recientemente agregado productos ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> No dude en limpiar la apariencia del control GridView. Algunas sugerencias incluyen el formateo muestre `UnitPrice` valor como moneda y el uso de fuentes y colores de fondo para mejorar la apariencia de la cuadrícula.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Paso 2: Crear una página de contenido para agregar nuevos productos

La siguiente tarea consiste en crear una página de contenido desde la que un usuario puede agregar un nuevo producto a la `Products` tabla. Agregue una nueva página de contenido a la `Admin` carpeta denominada `AddProduct.aspx`, lo que permite seguro para enlazarla a la `Site.master` página maestra. Figura 5 se muestra el Explorador de soluciones después de esta página se ha agregado al sitio Web.


[![Agregue una nueva página de ASP.NET a la carpeta Admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Figura 05**: agregar una nueva página de ASP.NET para la `Admin` carpeta ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


Recuerde que en la [ *especificar el título, etiquetas Meta y otros encabezados de HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial hemos creado una clase de página base personalizada denominada `BasePage` que generó el título de la página si fue no se establece explícitamente. Vaya a la `AddProduct.aspx` código subyacente de la página de clase y hacer que se derivan de `BasePage` (en lugar de desde `System.Web.UI.Page`).

Por último, actualizaremos el `Web.sitemap` archivo para incluir una entrada en esta lección. Agregue el siguiente marcado debajo de la `<siteMapNode>` para la lección problemas de nomenclatura de Id. de Control:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Como se muestra en la figura 6, la adición de este `<siteMapNode>` elemento se refleja en la lista de lecciones.

Vuelva a `AddProduct.aspx`. En el control de contenido para el `MainContent` ContentPlaceHolder, agregar un control DetailsView y asígnele el nombre `NewProduct`. Enlazar el DetailsView a un nuevo control SqlDataSource denominado `NewProductDataSource`. Al igual que con SqlDataSource en el paso 1, configurar al Asistente para que utilice la base de datos Northwind y elegir para especificar una instrucción SQL personalizada. DetailsView se usará para agregar elementos a la base de datos, es preciso especificar un `SELECT` instrucción y un `INSERT` instrucción. Utilice la siguiente `SELECT` consulta:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

A continuación, en la pestaña Insertar, agregue las siguientes `INSERT` instrucción:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Después de completar al Asistente para ir a la etiqueta inteligente de DetailsView y Active la casilla "Habilitar inserción". Esto agrega un CommandField a DetailsView con su `ShowInsertButton` propiedad establecida en True. Dado que este DetailsView se usará únicamente para insertar datos, establezca la DetailsView `DefaultMode` propiedad `Insert`.

Eso es todo lo! Vamos a probar esta página. Visite `AddProduct.aspx` a través de un explorador, escriba un nombre y el precio (consulte la figura 6).


[![Agregar un nuevo producto a la base de datos](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Figura 06**: agregar un nuevo producto a la base de datos ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


Después de escribir en el nombre y el precio del producto nuevo, haga clic en el botón Insertar. Esto hace que el formulario envíe de nuevo. En la del devolución de datos, el control SqlDataSource `INSERT` se ejecuta la instrucción; sus dos parámetros se rellenan con los valores introducidos por el usuario en dos controles de cuadro de texto de DetailsView. Lamentablemente, no hay ningún indicador visual que se ha producido una inserción. Sería estupendo tener un mensaje que aparece, confirma que se ha agregado un nuevo registro. Dejar este como un ejercicio para el lector. Además, después de agregar un nuevo registro de DetailsView GridView en la página maestra sigue mostrando los mismos cinco registros que antes; no se incluye el registro recién agregados. Examinaremos cómo solucionar este problema en los próximos pasos.

> [!NOTE]
> Además de agregar alguna forma de comentarios visuales que se ha realizado correctamente la inserción, se le animo también actualizar la interfaz de inserción de DetailsView para incluir la validación. Actualmente, no hay ninguna validación. Si un usuario escribe un valor no válido para el `UnitPrice` campo, como "demasiado costoso," se producirá una excepción en devolución de datos cuando el sistema intenta convertir esa cadena en un decimal. Para obtener más información acerca de cómo personalizar la inserción de la interfaz, consulte el [ *personalizar la interfaz de modificación de datos* tutorial](https://asp.net/learn/data-access/tutorial-20-vb.aspx) de mi [trabajar con la serie de tutoriales de datos](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Paso 3: Crear propiedades y métodos públicos en la página principal

En el paso 1 se agrega un control Web Label denominado `GridMessage` anteriormente GridView en la página maestra. Esta etiqueta sirve para mostrar un mensaje de forma opcional. Por ejemplo, después de agregar un nuevo registro a la `Products` tabla, tengamos que queremos que aparezca un mensaje que dice: "*ProductName* se ha agregado a la base de datos." En lugar de codificar el texto de esta etiqueta en la página maestra, que nos convenga el mensaje se puede personalizar la página de contenido.

Dado que el control de etiqueta se implementa como una variable de miembro protegido dentro de la página maestra no son accesibles directamente desde las páginas de contenido. Para poder trabajar con la etiqueta dentro de una página maestra desde la página de contenido (o, de hecho, cualquier control Web en la página maestra) es necesario crear una propiedad pública en la página maestra que expone el control Web o actúa como un proxy que puede ser una de sus propiedades  tiene acceso. Agregue la siguiente sintaxis para la clase de código subyacente de la página maestra para exponer la etiqueta `Text` propiedad:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Cuando se agrega un nuevo registro a la `Products` tabla desde una página de contenido la `RecentProducts` GridView en la página maestra debe volver a enlazar a su origen de datos subyacente. Volver a enlazar la llamada de GridView su `DataBind` método. Dado que GridView en la página maestra no está accesible mediante programación a las páginas de contenido, se debe crear un método público en la página maestra, cuando se llama, se vuelve a enlazar los datos en GridView. Agregue el método siguiente a la clase de código subyacente de la página maestra:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Con el `GridMessageText` propiedad y `RefreshRecentProductsGrid` método en su lugar, cualquier página de contenido puede mediante programación establecer o leer el valor de la `GridMessage` etiqueta `Text` propiedad o volver a enlazar los datos a la `RecentProducts` GridView. Paso 4 examina cómo obtener acceso a los métodos y propiedades públicas de la página maestra de una página de contenido.

> [!NOTE]
> No se olvide de marcar la página maestra propiedades y métodos como `Public`. Si se no explícitamente denotan estas propiedades y métodos como `Public`, no será accesibles desde la página de contenido.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Paso 4: Llamar a los miembros públicos de la página maestra desde una página de contenido

Ahora que la página maestra tiene las propiedades públicas necesarias y los métodos, estamos listos invocar estas propiedades y métodos de la `AddProduct.aspx` página de contenido. En concreto, es necesario establecer la página maestra `GridMessageText` propiedad y llame a su `RefreshRecentProductsGrid` método después de que el nuevo producto se ha agregado a la base de datos. Todos los controles de Web de los datos ASP.NET activan eventos justo antes y después de completar varias tareas, que facilitan a los desarrolladores de páginas realizar alguna acción mediante programación antes o después de la tarea. Por ejemplo, cuando el usuario final hace clic en el botón de insertar de DetailsView, en los postback la genera DetailsView su `ItemInserting` eventos antes de comenzar el flujo de trabajo de inserción. A continuación, se inserta el registro en la base de datos. A continuación, genera el DetailsView su `ItemInserted` eventos. Por lo tanto, para trabajar con la página maestra cuando se ha agregado el nuevo producto, crear un controlador de eventos para el DetailsView `ItemInserted` eventos.

Hay dos formas de una página de contenido mediante programación puede comunicarse con su página principal:

- Mediante el `Page.Master` propiedad, que devuelve una referencia débilmente tipada a la página maestra, o
- Especificar ruta de tipo o archivo de página maestra de la página a través de un `@MasterType` directiva; Esto agrega automáticamente una propiedad fuertemente tipados a la página denominada `Master`.

Vamos a examinar ambos enfoques.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Usar débilmente tipadas`Page.Master`propiedad

Todas las páginas web ASP.NET debe derivarse de la `Page` (clase), que se encuentra en la `System.Web.UI` espacio de nombres. El `Page` clase incluye una [ `Master` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) que devuelve una referencia a la página principal de la página. Si la página no tiene una página maestra `Master` devuelve `Nothing`.

El `Master` propiedad devuelve un objeto de tipo [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (también se encuentra en la `System.Web.UI` espacio de nombres) que es el tipo base del que derivan todas las páginas maestras. Por lo tanto, para usar propiedades o métodos públicos definidos en la página principal de nuestro sitio Web se debe convertir el `MasterPage` objeto devuelto desde el `Master` propiedad al tipo adecuado. Dado que se denomina nuestro archivo de página maestra `Site.master`, la clase de código subyacente se denominaba `Site`. Por lo tanto, el siguiente código convierte el `Page.Master` propiedad a una instancia de la `Site` clase.


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Ahora que tenemos convertir débilmente tipadas `Page.Master` propiedad para el tipo de sitio podemos hacer referencia a las propiedades y métodos específicos para el sitio. Como se muestra en la figura 7, la propiedad pública `GridMessageText` aparece en la lista desplegable de IntelliSense.


[![IntelliSense muestra nuestra página maestra propiedades y métodos públicos](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Figura 07**: IntelliSense muestra los métodos y propiedades públicas de nuestra página maestra ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> Si con el nombre de su archivo de página maestra `MasterPage.master` , a continuación, el nombre de clase de código subyacente de la página maestra es `MasterPage`. Esto puede conducir a código ambigua cuando realiza la conversión de tipo `System.Web.UI.MasterPage` a su `MasterPage` clase. En resumen, debe calificar totalmente el tipo que se va a convertir, que puede ser un poco complicado cuando se usa el modelo de proyecto de sitio Web. Mi sugerencia sería asegurarse de que al crear la página maestra que se denomina algo distinto de `MasterPage.master` o incluso mejor, crear una referencia a la página maestra fuertemente tipada.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Crear una referencia fuertemente tipado con el`@MasterType`directiva

Si observa detenidamente puede ver que la clase de código subyacente de una página ASP.NET es una clase parcial (tenga en cuenta el `Partial` palabra clave en la definición de clase). Clases parciales se introdujeron en C# y Visual Basic.NET Framework 2.0 y, en pocas palabras, permitir que los miembros de la clase que se defina en varios archivos. El archivo de clase de código subyacente - `AddProduct.aspx.vb`, por ejemplo: contiene el código que, el desarrollador de páginas, creamos. Además de nuestro código, el motor ASP.NET crea automáticamente un archivo de clase independiente con propiedades y controladores de eventos traducen el marcado declarativo en la jerarquía de clases de la página.

La generación automática de código que se produce cuando se visita una página ASP.NET prepara el terreno para algunas posibilidades interesantes y útiles en su lugar. En el caso de las páginas maestras, si se indican al motor ASP.NET página principal que está siendo utilizado por nuestra página de contenido genera fuertemente tipadas `Master` propiedad para nosotros.

Use la [ `@MasterType` directiva](https://msdn.microsoft.com/library/ms228274.aspx) para informar al motor ASP.NET de tipo de página maestra de la página de contenido. El `@MasterType` directiva puede aceptar el nombre de tipo de la página maestra o su ruta de acceso de archivo. Para especificar que la `AddProduct.aspx` página usa `Site.master` como su página principal, agregue la siguiente directiva a la parte superior de `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Esta directiva indica al motor ASP.NET para agregar una referencia fuertemente tipados a la página maestra a través de una propiedad denominada `Master`. Con el `@MasterType` directivas en su lugar, se puede llamar al `Site.master` maestro de la página Propiedades y métodos públicos directamente desde el `Master` propiedad sin todas las conversiones.

> [!NOTE]
> Si se omite la `@MasterType` directiva, la sintaxis `Page.Master` y `Master` devolver lo mismo: un objeto imprecisa con página maestra de la página. Si incluye el `@MasterType` , a continuación, la directiva `Master` devuelve una referencia fuertemente tipados a la página principal especificada. `Page.Master`, sin embargo, todavía se devuelve una referencia débilmente tipadas. Para obtener una visión más profunda en por qué este es el caso y cómo el `Master` propiedad se ha construido cuando la `@MasterType` se incluye la directiva, consulte [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)de entrada de blog [ `@MasterType` en ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Actualizar la página principal después de agregar un nuevo producto

Ahora que sabemos cómo invocar una página maestra propiedades y métodos públicos de una página de contenido, estamos listos actualizar la `AddProduct.aspx` página para que la página maestra se actualiza después de agregar un nuevo producto. Al principio del paso 4 se crea un controlador de eventos para el control DetailsView `ItemInserting` evento, que se ejecuta inmediatamente después de que el nuevo producto se ha agregado a la base de datos. Agregue el código siguiente al controlador de eventos:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

El código anterior usa tanto la imprecisa `Page.Master` propiedad y fuertemente tipadas `Master` propiedad. Tenga en cuenta que la `GridMessageText` propiedad se establece en "*ProductName* agregadas a cuadrícula..." Los valores de producto recién agregados son accesibles a través de la `e.Values` colección; como puede ver, el recién agregados `ProductName` valor se tiene acceso a través de `e.Values("ProductName")`.

La figura 8 muestra la `AddProduct.aspx` página inmediatamente después de un nuevo producto - de Scott Soda - se ha agregado a la base de datos. Tenga en cuenta que el nombre de producto recién agregado se indica en la etiqueta de la página maestra y que el control GridView se ha actualizado para incluir el producto y su precio.


[![La página maestra etiqueta y GridView mostrar el producto recién agregados](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Figura 08**: la página maestra etiqueta y GridView mostrar los productos Just-Added ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>Resumen

Lo ideal es que, una página maestra y las páginas de contenido son completamente independientes entre sí y no requieren ningún nivel de interacción. Aunque las páginas maestras y páginas de contenido deben estar diseñadas con ese objetivo en mente, hay una serie de escenarios comunes en los que una página de contenido debe comunicarse con su página maestra. Uno de los motivos más habituales se centra en la actualización de una parte determinada de la presentación de página maestra basándose en alguna acción que se hayan producido en la página de contenido.

Las buenas noticias son que resulta bastante sencillo para que una página de contenido interactuar mediante programación con su página maestra. Comience creando métodos o propiedades públicos en la página maestra que encapsulan la funcionalidad que necesita para invocar una página de contenido. A continuación, en la página de contenido, tener acceso a propiedades y métodos de la página maestra mediante débilmente tipadas `Page.Master` propiedad o use el `@MasterType` directiva para crear una referencia a la página maestra fuertemente tipada.

En el siguiente tutorial, examinaremos cómo hacer que la página maestra interactuar mediante programación con uno de sus páginas de contenido.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Obtener acceso y actualizar los datos en ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Páginas maestras de ASP.NET: Sugerencias, trucos y capturas](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` en ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Pasar información entre el contenido y páginas maestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabajar con datos en los tutoriales ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Zack Jones. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](control-id-naming-in-content-pages-vb.md)
> [Siguiente](interacting-with-the-content-page-from-the-master-page-vb.md)
