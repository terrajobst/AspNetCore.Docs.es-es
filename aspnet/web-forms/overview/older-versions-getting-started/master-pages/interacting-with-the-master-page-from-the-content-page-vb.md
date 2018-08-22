---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interactuar con la página maestra desde la página de contenido (VB) | Microsoft Docs
author: rick-anderson
description: Examina cómo llamar a métodos, establecer propiedades, etc. de la página maestra desde el código en la página de contenido.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 59a00305cdcaf41ac0b37649382b9c3dc9ce1b0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836084"
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interactuar con la página maestra desde la página de contenido (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) o [descargar PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Examina cómo llamar a métodos, establecer propiedades, etc. de la página maestra desde el código en la página de contenido.


## <a name="introduction"></a>Introducción

En el transcurso de los cinco últimos tutoriales hemos examinado cómo crear una página maestra, definir las áreas de contenido, enlazar las páginas de ASP.NET a una página maestra y definir el contenido específico de la página. Cuando un visitante solicita una página de contenido determinada, el contenido y el marcado de las páginas maestras se fusionan en tiempo de ejecución, lo que resulta en la representación de una jerarquía de controles unificada. Por lo tanto, ya hemos visto una manera en que pueden interactuar a la página principal y uno de sus páginas de contenido: la página de contenido se detalla el marcado para transfuse en controles ContentPlaceHolder de la página maestra.

Lo que aún es necesario examinar es cómo pueden interactuar mediante programación la página maestra y la página de contenido. Además de definir el marcado para controles ContentPlaceHolder de la página principal, una página de contenido también puede asignar valores a propiedades públicas de sus páginas principales e invocar sus métodos públicos. De forma similar, una página maestra puede interactuar con las páginas de contenido. Aunque es menos frecuente que la interacción entre sus marcas declarativas interacción mediante programación entre una página maestra y contenida, hay muchos escenarios donde es necesario dicha interacción mediante programación.

En este tutorial se examina cómo una página de contenido mediante programación puede interactuar con su página maestra; en el siguiente tutorial veremos cómo la página principal de forma similar puede interactuar con las páginas de contenido.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Ejemplos de programación interacción entre una página de contenido y su página principal

Cuando una región determinada de una página debe configurarse en la página por página, usamos un control ContentPlaceHolder. Pero, ¿necesitan situaciones donde la mayoría de páginas necesario para emitir un resultado determinado, pero un pequeño número de páginas personalizarlo para mostrar algo más? Un ejemplo de esto, lo que hemos visto en el [ *varios ContentPlaceHolders y contenido predeterminado* ](multiple-contentplaceholders-and-default-content-vb.md) implica el tutorial, mostrar la interfaz de inicio de sesión en cada página. Aunque la mayoría de las páginas debe incluir una interfaz de inicio de sesión, se debe suprimir una serie de páginas, como: la página de inicio de sesión principal (`Login.aspx`); la página Crear una cuenta; y otras páginas que solo se puede acceder a los usuarios autenticados. El [ *varios ContentPlaceHolders y contenido predeterminado* ](multiple-contentplaceholders-and-default-content-vb.md) tutorial le ha mostrado cómo definir el contenido predeterminado para un ContentPlaceHolder en la página maestra y, a continuación, cómo invalidarlo en las páginas donde el no se quería contenido predeterminado.

Otra opción es crear una propiedad pública o un método en la página maestra que indica si se debe mostrar u ocultar la interfaz de inicio de sesión. Por ejemplo, la página maestra puede incluir una propiedad pública denominada `ShowLoginUI` cuyo valor se usa para establecer el `Visible` propiedad del control de inicio de sesión en la página maestra. Las páginas de contenido donde se debe suprimir la interfaz de usuario de inicio de sesión, a continuación, pueden establecer mediante programación el `ShowLoginUI` propiedad `False`.

Quizás el ejemplo más común de contenido y la interacción de la página principal se produce cuando los datos se muestran en la página maestra debe actualizarse una vez que haya llegado alguna acción en la página de contenido. Considere la posibilidad de una página maestra que incluye un control GridView que muestra los cinco más recientemente agregado registros de una tabla de base de datos determinada y que uno de sus páginas de contenido incluye una interfaz para agregar nuevos registros a la misma tabla.

Cuando un usuario visita la página para agregar un nuevo registro, ve que las cinco agregan recientemente registros mostrados en la página maestra. Después de rellenar los valores de columnas del nuevo registro, envía el formulario. Suponiendo que el control GridView en la página principal tiene su `EnableViewState` propiedad establecida en True (valor predeterminado), su contenido se vuelve a cargar del estado de vista y, por lo tanto, se muestran los mismos cinco registros, aunque un registro más reciente se acaba de agregar a la base de datos. Esto puede confundir al usuario.

> [!NOTE]
> Incluso si se deshabilita el estado de vista de GridView para que vuelve a enlazar al origen de datos subyacente en cada devolución de datos, lo todavía no mostrará el registro recién agregado porque los datos se enlazan a la GridView anteriormente en el ciclo de vida de la página que cuando se agrega el nuevo registro a la datab instancia de ase.


Para solucionar este problema para que el registro recién agregada se muestra en la página principal de GridView en el que necesitamos indicar a la GridView para volver a enlazar al origen de datos de postback *después* se ha agregado el nuevo registro a la base de datos. Esto requiere la interacción entre el contenido y páginas maestras porque la interfaz para agregar que el nuevo registro (y sus controladores de eventos) se encuentran en la página de contenido, pero el control GridView que debe actualizarse en la página maestra.

Dado que actualizar la presentación de la página maestra desde un controlador de eventos en la página de contenido es una de las necesidades más comunes para el contenido y la interacción de la página maestra, vamos a examinar en este tema con más detalle. La descarga de este tutorial incluye una base de datos de Microsoft SQL Server 2005 Express Edition con nombre `NORTHWIND.MDF` en el sitio de Web `App_Data` carpeta. La base de datos Northwind almacena información de ventas de una compañía ficticia, Northwind Traders, empleados y producto.

Paso 1 recorre mostrar las cinco más recientemente habían agregado productos en un control GridView en la página maestra. Paso 2 crea una página de contenido para agregar nuevos productos. Paso 3 se examina cómo crear propiedades y métodos públicos en la página maestra y paso 4 se muestra cómo interactuar mediante programación con estas propiedades y métodos de la página de contenido.

> [!NOTE]
> En este tutorial no profundizar en los detalles del trabajo con datos en ASP.NET. Los pasos de configuración de la página principal para mostrar los datos y la página de contenido para insertar datos son completos e implementaciones fáciles y aún. Para una visión más profunda de mostrar y la inserción de datos y cómo usar los controles SqlDataSource y GridView, consulte los recursos en la sección Lecturas adicionales al final de este tutorial.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Paso 1: Mostrar las cinco más recientemente agregado productos en la página maestra

Abra la página maestra Site.master y agregue una etiqueta y un control GridView a la `leftContent` `<div>`. Limpiar la etiqueta `Text` establecer la propiedad, su `EnableViewState` propiedad `False`y su `ID` propiedad a `GridMessage`; establecer la GridView `ID` propiedad a `RecentProducts`. A continuación, desde el diseñador, expanda la etiqueta inteligente de GridView y elija enlazarlo a un origen de datos. Esto inicia al Asistente para configuración de origen de datos. Dado que la base de datos Northwind en el `App_Data` carpeta es una base de datos de Microsoft SQL Server, optar por crear un SqlDataSource seleccionando (consulte la figura 1); nombre SqlDataSource `RecentProductsDataSource`.


[![Enlazar el control GridView a un Control SqlDataSource denominado RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Figura 01**: enlazar el control GridView a un Control SqlDataSource denominado `RecentProductsDataSource` ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


El siguiente paso nos pide para especificar qué base de datos al que conectarse. Elija la `NORTHWIND.MDF` base de datos de archivo en la lista desplegable y haga clic en siguiente. Dado que esta es la primera vez que hemos usado esta base de datos, el asistente ofrecerá almacenar la cadena de conexión en `Web.config`. Tiene almacenar la cadena de conexión con el nombre `NorthwindConnectionString`.


[![Conectarse a la base de datos Northwind](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Figura 02**: conectarse a la base de datos Northwind ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


El Asistente para configurar orígenes de datos proporciona dos significa que se puede especificar la consulta utilizada para recuperar los datos:

- Mediante la especificación de una instrucción SQL personalizada o un procedimiento almacenado, o
- Al seleccionar una tabla o vista y, a continuación, especificar las columnas que se va a devolver

Puesto que deseamos devolver que solo los cinco agregan recientemente a productos, necesitamos especificar una instrucción SQL personalizada. Use el siguiente `SELECT` consulta:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

El `TOP 5` palabra clave devuelve solo los primeros cinco registros de la consulta. El `Products` clave principal de la tabla, `ProductID`, es un `IDENTITY` columna, lo que nos garantiza que cada nuevo producto que se agregan a la tabla tendrá un valor mayor que la entrada anterior. Por lo tanto, ordenar los resultados por `ProductID` en orden descendente devuelve los productos a partir de la última creadas nuevamente.


[![Devolver los cinco productos agregados recientemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Figura 03**: devolver los cinco más recientemente agregado productos ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


Después de completar el asistente, Visual Studio genera dos BoundFields del control GridView mostrar el `ProductName` y `UnitPrice` campos devueltos desde la base de datos. En este punto marcado declarativo de la página maestra debe incluir marcado similar al siguiente:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Como puede ver, contiene el marcado: el control Web Label (`GridMessage`); el control GridView `RecentProducts`, con dos BoundFields; y un control SqlDataSource que devuelve las cinco agregan recientemente productos.

Con esto GridView que creó y su control SqlDataSource configurado, visite el sitio Web a través de un explorador. Como se muestra en la figura 4, verá una cuadrícula en la esquina inferior izquierda que se enumera los cinco más recientemente agregado productos.


[![El control GridView muestra los cinco productos agregados recientemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Figura 04**: GridView muestra los cinco más recientemente agregado productos ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> No dude en limpiar la apariencia del control GridView. Algunas sugerencias incluyen el formateo muestran `UnitPrice` valor como una moneda y el uso de fuentes y colores de fondo para mejorar la apariencia de la cuadrícula.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Paso 2: Creación de una página de contenido para agregar nuevos productos

La siguiente tarea consiste en crear una página de contenido desde el que un usuario puede agregar un nuevo producto a la `Products` tabla. Agregue una nueva página de contenido para el `Admin` carpeta denominada `AddProduct.aspx`y asegúrese que se va a enlazar la `Site.master` página maestra. Figura 5 muestra el Explorador de soluciones después de esta página se ha agregado al sitio Web.


[![Agregue una nueva página de ASP.NET a la carpeta Admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Figura 05**: agregar una nueva página de ASP.NET para la `Admin` carpeta ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


Recuerde que en el [ *especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial hemos creado una clase de página base personalizada denominada `BasePage` que generó el título de la página si fuese no se establece explícitamente. Vaya a la `AddProduct.aspx` código subyacente de la página de clase y hacer que se derivan de `BasePage` (en lugar de desde `System.Web.UI.Page`).

Por último, actualice el `Web.sitemap` archivo para incluir una entrada en esta lección. Agregue el siguiente marcado debajo de la `<siteMapNode>` de la lección de problemas de nomenclatura de Id. de Control:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Como se muestra en la figura 6, la adición de este `<siteMapNode>` elemento se refleja en la lista de las lecciones.

Vuelva a `AddProduct.aspx`. En el control de contenido para el `MainContent` ContentPlaceHolder, agregue un control DetailsView y asígnele el nombre `NewProduct`. Enlazar un control SqlDataSource nuevo denominado DetailsView `NewProductDataSource`. Al igual que con SqlDataSource en el paso 1, configurar al Asistente para que utilice la base de datos Northwind y optar por especificar una instrucción SQL personalizada. Como DetailsView se usará para agregar elementos a la base de datos, se debe especificar tanto un `SELECT` instrucción y un `INSERT` instrucción. Use el siguiente `SELECT` consulta:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

A continuación, en la pestaña Insertar, agregue las siguientes `INSERT` instrucción:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Después de completar al asistente vaya a la etiqueta inteligente de DetailsView y Active la casilla "Habilitar inserción". Esto agrega un CommandField a DetailsView con su `ShowInsertButton` propiedad establecida en True. Porque se usará este DetailsView únicamente para insertar datos, establezca el DetailsView `DefaultMode` propiedad `Insert`.

Eso es todo esto! Vamos a probar esta página. Visite `AddProduct.aspx` a través de un explorador, escriba el nombre y precio (consulte la figura 6).


[![Agregar un nuevo producto a la base de datos](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Figura 06**: agregar un nuevo producto a la base de datos ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


Después de escribir el nombre y el precio del producto de nuevo, haga clic en el botón Insertar. Esto hace que el formulario de devolución de datos. En la del devolución de datos, el control SqlDataSource `INSERT` se ejecuta la instrucción; sus dos parámetros se rellenan con los valores introducidos por el usuario en dos controles de cuadro de texto de DetailsView. Lamentablemente, no hay ningún indicador visual que se ha producido una inserción. Sería bueno tener un mensaje de muestra, que confirma que se ha agregado un nuevo registro. Dejar como un ejercicio para el lector. Además, después de agregar un nuevo registro de DetailsView GridView en la página principal sigue mostrando los mismos cinco registros que antes; no incluye el registro recién agregado. Examinaremos cómo solucionar este problema en los próximos pasos.

> [!NOTE]
> Además de agregar algún tipo de comentarios visuales que se ha realizado correctamente la inserción, me gustaría incentivarlo actualizar también la interfaz de inserción de DetailsView para incluir la validación. Actualmente, no hay ninguna validación. Si un usuario escribe un valor no válido para el `UnitPrice` campo, como "demasiado costoso," se producirá una excepción en el postback cuando el sistema intenta convertir esa cadena en un decimal. Para obtener más información sobre la personalización de la inserción de la interfaz, consulte el [ *personalizar la interfaz de modificación de datos* tutorial](https://asp.net/learn/data-access/tutorial-20-vb.aspx) desde mi [trabajar con la serie de tutoriales de datos](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Paso 3: Creación de propiedades y métodos públicos en la página maestra

En el paso 1 se ha agregado un control etiqueta Web denominado `GridMessage` por encima del control GridView en la página maestra. Esta etiqueta está pensada para mostrar opcionalmente un mensaje. Por ejemplo, después de agregar un nuevo registro a la `Products` tabla, deseamos mostrar un mensaje que dice: "*ProductName* se ha agregado a la base de datos." En lugar de codificar el texto de esta etiqueta en la página maestra, podríamos llevar el mensaje se puede personalizar la página de contenido.

Dado que el control de etiqueta se implementa como una variable de miembro protegido dentro de la página maestra no son accesibles directamente desde las páginas de contenido. Para trabajar con la etiqueta dentro de una página maestra desde la página de contenido (o, de hecho, cualquier control Web en la página maestra) se necesita crear una propiedad pública en la página maestra que expone el control Web o actúa como un proxy mediante el cual puede ser uno de sus propiedades  obtener acceso a. Agregue la siguiente sintaxis para la clase de código subyacente de la página maestra para exponer la etiqueta `Text` propiedad:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Cuando se agrega un nuevo registro a la `Products` tabla desde una página de contenido la `RecentProducts` GridView en la página maestra debe volver a enlazarse a su origen de datos subyacente. Para volver a enlazar la llamada de GridView su `DataBind` método. Dado que el control GridView en la página principal no es accesible mediante programación a las páginas de contenido, se debe crear un método público en la página maestra, cuando se llama, vuelve a enlazar los datos en GridView. Agregue el método siguiente a la clase de código subyacente de la página maestra:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Con el `GridMessageText` propiedad y `RefreshRecentProductsGrid` método en su lugar, cualquier página de contenido puede mediante programación establecer o leer el valor de la `GridMessage` la etiqueta `Text` propiedad o volver a enlazar los datos a la `RecentProducts` GridView. Paso 4, examina cómo acceder a los métodos y propiedades públicas de la página maestra desde una página de contenido.

> [!NOTE]
> No se olvide de marcar las propiedades y los métodos como la página maestra `Public`. Si se no explícitamente denotan estas propiedades y métodos como `Public`, no será accesibles desde la página de contenido.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Paso 4: Llamar a miembros públicos de la página maestra desde una página de contenido

Ahora que la página maestra tiene los métodos y propiedades públicas es necesarias, ya estamos listos para invocar estas propiedades y métodos de la `AddProduct.aspx` página de contenido. En concreto, es necesario establecer la página maestra `GridMessageText` propiedad y llame a su `RefreshRecentProductsGrid` método después de que se ha agregado el nuevo producto a la base de datos. Todos los controles de Web de datos ASP.NET activan eventos inmediatamente antes y después de completar varias tareas, lo que facilitan a los desarrolladores de páginas realizar alguna acción mediante programación antes o después de la tarea. Por ejemplo, cuando el usuario final hace clic en el botón de inserción de DetailsView, en postback DetailsView provoca su `ItemInserting` eventos antes de comenzar el flujo de trabajo de inserción. A continuación, inserta el registro en la base de datos. A continuación, genera DetailsView su `ItemInserted` eventos. Por lo tanto, para poder trabajar con la página principal después de que se ha agregado el nuevo producto, crear un controlador de eventos para el DetailsView `ItemInserted` eventos.

Hay dos maneras en que una página de contenido mediante programación puede comunicarse con su página principal:

- Mediante el `Page.Master` propiedad, que devuelve una referencia fuertemente tipado a la página maestra, o
- Especificar ruta de tipo o el archivo de página maestra de la página a través de un `@MasterType` directiva; Esto agrega automáticamente una propiedad fuertemente tipados a la página llamada `Master`.

Vamos a examinar ambos enfoques.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Usar débilmente tipadas`Page.Master`propiedad

Todas las páginas web ASP.NET debe derivar de la `Page` (clase), que se encuentra en la `System.Web.UI` espacio de nombres. El `Page` clase incluye un [ `Master` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) que devuelve una referencia a la página principal de la página. Si la página no tiene una página maestra `Master` devuelve `Nothing`.

El `Master` propiedad devuelve un objeto de tipo [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (también se encuentra en la `System.Web.UI` espacio de nombres) que es el tipo base del que derivan todas las páginas maestras. Por lo tanto, para usar propiedades o métodos públicos definidos en la página principal del sitio Web nos debemos convertir la `MasterPage` objeto devuelto desde el `Master` propiedad al tipo adecuado. Dado que hemos llamado a nuestro archivo de página maestra `Site.master`, la clase de código subyacente se denominaba `Site`. Por lo tanto, el siguiente código convierte el `Page.Master` propiedad a una instancia de la `Site` clase.


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Ahora que hemos convertido débilmente tipadas `Page.Master` propiedad para el tipo de sitio podemos hacer referencia a las propiedades y métodos específicos de sitio. Como se muestra en la figura 7, la propiedad pública `GridMessageText` aparece en la lista desplegable de IntelliSense.


[![IntelliSense muestra los métodos y propiedades públicas de nuestra página maestra](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Figura 07**: IntelliSense muestra los métodos y propiedades públicas de nuestra página maestra ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> Si el nombre de su archivo de página maestra `MasterPage.master` , a continuación, el nombre de clase de código subyacente de la página maestra es `MasterPage`. Esto puede dar lugar a código ambigua cuando se realiza la conversión de tipo `System.Web.UI.MasterPage` a su `MasterPage` clase. En resumen, debe usar el nombre completo del tipo que se va a convertir, que puede ser un poco complicado cuando se usa el modelo de proyecto de sitio Web. Mi sugerencia sería asegurarse de que al crear la página maestra asígnele el nombre algo distinto `MasterPage.master` o, mejor aún, cree una referencia fuertemente tipada a la página maestra.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Creación de una referencia fuertemente tipado con el`@MasterType`directiva

Si lo examina detenidamente puede ver que la clase de código subyacente de una página ASP.NET es una clase parcial (tenga en cuenta el `Partial` palabra clave en la definición de clase). Las clases parciales se presentaron en C# y Visual Basic con.NET Framework 2.0 y, en resumen, permiten para que los miembros de una clase se define en varios archivos. El archivo de clase de código subyacente - `AddProduct.aspx.vb`, por ejemplo, contiene el código que, el desarrollador de páginas, creamos. Además de nuestro código, el motor ASP.NET crea automáticamente un archivo de clase independiente con propiedades y controladores de eventos traducen el marcado declarativo en la jerarquía de clases de la página.

La generación automática de código que se produce cada vez que se visite una página ASP.NET prepara el terreno para algunas posibilidades bastante interesantes y útiles. En el caso de las páginas maestras, si se indican al motor ASP.NET nuestra página de contenido está usando la página principal que genera fuertemente tipadas `Master` propiedad para nosotros.

Use la [ `@MasterType` directiva](https://msdn.microsoft.com/library/ms228274.aspx) para informar al motor de ASP.NET de tipo de página principal de la página de contenido. El `@MasterType` directiva puede aceptar el nombre de tipo de la página maestra o su ruta de acceso de archivo. Para especificar que el `AddProduct.aspx` página usa `Site.master` como su página principal, agregue la siguiente directiva a la parte superior de `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Esta directiva indica al motor ASP.NET para agregar una referencia fuertemente tipada a la página principal a través de una propiedad denominada `Master`. Con el `@MasterType` la directiva en su lugar, podemos llamar a la `Site.master` dominar la página Propiedades y métodos públicos directamente a través del `Master` propiedad sin todas las conversiones.

> [!NOTE]
> Si se omite el `@MasterType` directiva, la sintaxis `Page.Master` y `Master` devolver lo mismo: un objeto fuertemente tipado a página principal de la página. Si incluye el `@MasterType` , a continuación, la directiva `Master` devuelve una referencia fuertemente tipada a la página principal especificada. `Page.Master`, sin embargo, sigue devolviendo una referencia fuertemente tipado. Para una visión más completa de por qué este es el caso y cómo el `Master` se construye la propiedad cuando la `@MasterType` se incluye la directiva, consulte [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)de entrada de blog [ `@MasterType` en ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Actualización de la página principal después de agregar un nuevo producto

Ahora que sabemos cómo invocar los métodos desde una página de contenido y las propiedades públicas de una página maestra, estamos listos actualizar la `AddProduct.aspx` página para que se actualice la página principal después de agregar un nuevo producto. Al principio del paso 4, hemos creado un controlador de eventos para el control DetailsView `ItemInserting` evento, que se ejecuta inmediatamente después de que se ha agregado el nuevo producto a la base de datos. Agregue el siguiente código a ese controlador de eventos:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

El código anterior usa ambos el débilmente tipadas `Page.Master` propiedad y fuertemente tipadas `Master` propiedad. Tenga en cuenta que el `GridMessageText` propiedad está establecida en "*ProductName* agregado a la cuadrícula..." Los valores del producto recién agregados son accesibles a través de la `e.Values` colección; como puede ver, el recién agregadas por `ProductName` valor se tiene acceso a través de `e.Values("ProductName")`.

La figura 8 muestra el `AddProduct.aspx` página inmediatamente después de un nuevo producto - de Scott Soda - se ha agregado a la base de datos. Tenga en cuenta que el nombre de producto recién agregado se indica en la etiqueta de la página maestra y que el control GridView se ha actualizado para incluir el producto y su precio.


[![Etiqueta y GridView muestra el producto recién agregada de la página maestra](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Figura 08**: etiqueta y GridView de la página principal muestra el producto Just-Added ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>Resumen

Lo ideal es que, una página maestra y las páginas de contenido son completamente independientes entre sí y no requieren ningún nivel de interacción. Aunque las páginas maestras y las páginas de contenido deben diseñarse con ese objetivo en mente, hay una serie de escenarios comunes en el que una página de contenido debe interactuar con su página principal. Uno de los motivos más comunes se centra en la actualización de una parte determinada de la presentación de página maestra en función de alguna acción que se ha producido en la página de contenido.

La buena noticia es que es relativamente sencillo tener una página de contenido interactuar mediante programación con su página principal. Comience creando métodos o propiedades públicos en la página maestra que encapsulan la funcionalidad que debe invocarse mediante una página de contenido. A continuación, en la página de contenido, tener acceso a propiedades y métodos de la página maestra mediante débilmente tipadas `Page.Master` propiedad o use el `@MasterType` directiva para crear una referencia fuertemente tipada a la página maestra.

En el siguiente tutorial se examina cómo hacer que la página maestra interactuar mediante programación con uno de sus páginas de contenido.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Acceder y actualizar datos en ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Páginas maestras en ASP.NET: Sugerencias, trucos y capturas](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` en ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Pasar información entre el contenido y páginas maestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabajar con datos en los tutoriales ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [ *Sams Teach usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Zack Jones. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](control-id-naming-in-content-pages-vb.md)
> [Siguiente](interacting-with-the-content-page-from-the-master-page-vb.md)
