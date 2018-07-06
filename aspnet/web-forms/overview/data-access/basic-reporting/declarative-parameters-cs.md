---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
title: Parámetros declarativos (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial le explicaremos cómo usar un parámetro establecido en un valor codificado de forma rígida para seleccionar los datos que se va a mostrar en un control DetailsView.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 603c9bd3-b895-4ec6-853b-0c81ff36d580
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
msc.type: authoredcontent
ms.openlocfilehash: 06577628d2b502df2854a83af564a0c0d402e9bd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839430"
---
<a name="declarative-parameters-c"></a>Parámetros declarativos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_5_CS.exe) o [descargar PDF](declarative-parameters-cs/_static/datatutorial05cs1.pdf)

> En este tutorial le explicaremos cómo usar un parámetro establecido en un valor codificado de forma rígida para seleccionar los datos que se va a mostrar en un control DetailsView.


## <a name="introduction"></a>Introducción

En el [último tutorial](displaying-data-with-the-objectdatasource-cs.md) contemplamos mostrar datos con los controles GridView, DetailsView y FormView enlazados a un control ObjectDataSource que invoca la `GetProducts()` método desde el `ProductsBLL` clase. El `GetProducts()` método devuelve una tabla de datos fuertemente tipados rellenado con todos los registros de la base de datos Northwind `Products` tabla. El `ProductsBLL` clase contiene métodos adicionales para devolver subconjuntos solo de los productos - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, y `GetProductsBySupplierID(supplierID)`. Estos tres métodos esperan un parámetro de entrada que indica cómo filtrar la información de producto devuelto.

ObjectDataSource puede usarse para invocar métodos que esperan parámetros de entrada, pero para ello, debemos especificar dónde proceden los valores para estos parámetros. Los valores de parámetro puede ser codificada de forma rígida o puede proceder de diversos orígenes dinámicos, incluidos: los valores de cadena de consulta, variables de sesión, el valor de propiedad de un control Web en la página o a otros usuarios.

En este tutorial comenzar, creemos que ilustra cómo usar un parámetro establecido en un valor codificado de forma rígida. En concreto, examinaremos agregar DetailsView en la página que muestra información acerca de un producto concreto, es decir, de Chef Antón tártara, que tiene un `ProductID` de 5. A continuación, veremos cómo establecer el valor del parámetro en función de un control Web. En concreto, vamos a usar un cuadro de texto para permitir que el usuario escriba en un país, tras el cual puede haga clic en un botón para ver la lista de proveedores que residen en ese país.

## <a name="using-a-hard-coded-parameter-value"></a>Uso de un valor de parámetro codificado de forma rígida

Para el primer ejemplo, empiece por agregar un control DetailsView en la `DeclarativeParams.aspx` página en el `BasicReporting` carpeta. En las etiquetas inteligentes de DetailsView, seleccione &lt;nuevo origen de datos&gt; en la lista desplegable lista y elija Agregar un origen ObjectDataSource.


[![Agregar un origen ObjectDataSource a la página](declarative-parameters-cs/_static/image2.png)](declarative-parameters-cs/_static/image1.png)

**Figura 1**: agregar un origen ObjectDataSource a la página ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image3.png))


Esto iniciará automáticamente el Asistente de Elegir origen de datos del control ObjectDataSource. Seleccione el `ProductsBLL` clase a partir de la primera pantalla del asistente.


[![Seleccione la clase ProductsBLL](declarative-parameters-cs/_static/image5.png)](declarative-parameters-cs/_static/image4.png)

**Figura 2**: seleccione el `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image6.png))


Puesto que queremos mostrar información sobre un producto determinado que deseamos utilizar el `GetProductByProductID(productID)` método.


[![Elija el método GetProductByProductID(productID)](declarative-parameters-cs/_static/image8.png)](declarative-parameters-cs/_static/image7.png)

**Figura 3**: elija la `GetProductByProductID(productID)` método ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image9.png))


Dado que el método seleccionamos incluye un parámetro, hay una pantalla más para que el asistente, donde se nos pide que defina el valor que se usará para el parámetro. La lista de la izquierda muestra todos los parámetros para el método seleccionado. Para `GetProductByProductID(productID)` solo hay un `productID`. Podemos especificar el valor para el parámetro seleccionado en la parte derecha. La lista desplegable de origen de parámetro enumera los distintos orígenes posibles para el valor del parámetro. Puesto que deseamos especificar un valor codificado de forma rígida de 5 para el `productID` parámetro, deje el origen del parámetro como Ninguno y escriba 5 en el cuadro de texto DefaultValue.


[![Un Hard-Coded parámetro de valor de 5 se utilizará para el parámetro productID](declarative-parameters-cs/_static/image11.png)](declarative-parameters-cs/_static/image10.png)

**Figura 4**: A Hard-Coded parámetro de valor de 5 se utilizará para el `productID` parámetro ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image12.png))


Después de completar el Asistente para configurar orígenes de datos, que incluye marcado declarativo del control ObjectDataSource un `Parameter` objeto en el `SelectParameters` colección para cada uno de los parámetros de entrada esperados por el método definido en el `SelectMethod` propiedad. Puesto que el método que usamos en este ejemplo espera sólo un único parámetro de entrada, `parameterID`, hay sólo una entrada aquí. El `SelectParameters` colección puede contener cualquier clase que deriva el `Parameter` clase en el `System.Web.UI.WebControls` espacio de nombres. Para la base de los valores de parámetro codificado de forma rígida `Parameter` se usa la clase, pero para el otro parámetro de origen opciones una derivada `Parameter` se utiliza la clase; también puede crear sus propios [tipos de parámetros personalizados](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), si es necesario.

[!code-aspx[Main](declarative-parameters-cs/samples/sample1.aspx)]

> [!NOTE]
> Si, siga estos pasos en su propio equipo el marcado declarativo verá en este momento puede incluir los valores para el `InsertMethod`, `UpdateMethod`, y `DeleteMethod` propiedades, así como `DeleteParameters`. Asistente de Elegir origen de datos de ObjectDataSource especifica automáticamente los métodos de la `ProductBLL` que se usará para insertar, actualizar y eliminar, por lo que, a menos que se los horizontal desactiva explícitamente, deberá incluirse en el marcado anterior.


Al visitar esta página, los datos de control Web invocará la ObjectDataSource `Select` método, que llamará el `ProductsBLL` la clase `GetProductByProductID(productID)` método utilizando el valor codificado de forma rígida de 5 para el `productID` parámetro de entrada. El método devolverá fuertemente tipadas `ProductDataTable` objeto que contiene una sola fila con información acerca tártara de Chef Antón (el producto con `ProductID` 5).


[![Se muestran tártara del información sobre Chef Antón](declarative-parameters-cs/_static/image14.png)](declarative-parameters-cs/_static/image13.png)

**Figura 5**: se muestran tártara del información sobre Chef Antón ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Establecer el valor del parámetro con el valor de propiedad de un Control Web

Parámetro de ObjectDataSource también se pueden establecer los valores según el valor de un control Web en la página. Para ilustrar esto, vamos a tener un control GridView que muestra todos los proveedores que se encuentran en un país o región especificado por el usuario. Para realizar este tutorial de inicio mediante la adición de un cuadro de texto a la página en la que el usuario puede escribir un nombre de país. Establecer este control de cuadro de texto `ID` propiedad `CountryName`. Agregue también un control de botón Web.


[![Agregue un cuadro de texto a la página con el Id. de CountryName](declarative-parameters-cs/_static/image17.png)](declarative-parameters-cs/_static/image16.png)

**Figura 6**: agregar un cuadro de texto a la página con `ID` `CountryName` ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image18.png))


A continuación, agregue un control GridView a la página y, en la etiqueta inteligente, optar por agregar un nuevo origen ObjectDataSource. Puesto que deseamos mostrar seleccionar información de proveedor la `SuppliersBLL` clase a partir de la pantalla del asistente primera. En la segunda pantalla, elija el `GetSuppliersByCountry(country)` método.


[![Elija el método GetSuppliersByCountry(country)](declarative-parameters-cs/_static/image20.png)](declarative-parameters-cs/_static/image19.png)

**Figura 7**: elija la `GetSuppliersByCountry(country)` método ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image21.png))


Puesto que el `GetSuppliersByCountry(country)` método tiene un parámetro de entrada, una vez más, el asistente incluye una pantalla final para elegir el valor del parámetro. Esta vez, establezca el parámetro source al Control. Esto rellenará la lista desplegable iDControl con los nombres de los controles en la página; Seleccione el `CountryName` control en la lista. Cuando primero se visita la página el `CountryName` TextBox estará en blanco, por lo que se devuelve ningún resultado y se muestra nada. Si desea mostrar algunos resultados de forma predeterminada, establezca el cuadro de texto DefaultValue en consecuencia.


[![Establece el valor del parámetro en el valor del Control CountryName](declarative-parameters-cs/_static/image23.png)](declarative-parameters-cs/_static/image22.png)

**Figura 8**: establece el valor del parámetro en el `CountryName` valor de Control ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image24.png))


Marcado declarativo de ObjectDataSource difiere ligeramente de nuestro primer ejemplo, mediante un [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) en lugar del estándar `Parameter` objeto. Un `ControlParameter` tiene propiedades adicionales para especificar el `ID` del control Web y el valor de propiedad que se utilizará para el parámetro (`PropertyName`). El Asistente para configurar orígenes de datos fue lo suficientemente inteligente como para determinar que, para un cuadro de texto, probablemente deseará usar la `Text` propiedad para el valor del parámetro. Si, sin embargo, desea utilizar un valor de propiedad diferente desde el control Web puede cambiar el `PropertyName` valor aquí o haciendo clic en el vínculo "Mostrar propiedades avanzadas" en el asistente.

[!code-aspx[Main](declarative-parameters-cs/samples/sample2.aspx)]

Cuando se visita la página por primera vez el `CountryName` cuadro de texto está vacía. El ObjectDataSource `Select` todavía se invoca el método por el control GridView, pero un valor de `null` se pasa a la `GetSuppliersByCountry(country)` método. Convierte el TableAdapter los `null` en una base de datos `NULL` valor (`DBNull.Value`), pero la consulta usada por el `GetSuppliersByCountry(country)` método se escribe de manera que no devuelve cualquiera los valores cuando un `NULL` valor especificado para el `@CategoryID`parámetro. En pocas palabras, no hay proveedores se devuelven.

Una vez que el visitante entra en un país, sin embargo y hace clic en el botón Mostrar proveedores para que se produzca un postback, ObjectDataSource `Select` se requiere el método, pasando el control de cuadro de texto `Text` valor como el `country` parámetro.


[![Se muestran los proveedores de Canadá](declarative-parameters-cs/_static/image26.png)](declarative-parameters-cs/_static/image25.png)

**Figura 9**: se muestran los proveedores de Canadá ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Mostrar todos los proveedores de forma predeterminada

En su lugar de mostrar ninguno de los proveedores cuando se ven en primer lugar la página que podríamos desear mostrar *todas* proveedores en primer lugar, lo que permite al usuario reducir la lista especificando un nombre de país en el cuadro de texto. Cuando el cuadro de texto está vacía, el `SuppliersBLL` la clase `GetSuppliersByCountry(country)` método se pasa un `null` valor para su *`country`* parámetro de entrada. Esto `null` valor, a continuación, se han transmitido a la DAL `GetSupplierByCountry(country)` método, donde se traduce a una base de datos `NULL` valor para el `@Country` parámetro en la consulta siguiente:

[!code-sql[Main](declarative-parameters-cs/samples/sample3.sql)]

La expresión `Country = NULL` siempre devuelve False, incluso para registros cuya `Country` columna tiene un `NULL` valor; por lo tanto, se devuelve ningún registro.

Para devolver *todos los* proveedores cuando el país del cuadro de texto está vacío, podemos aumentamos la `GetSuppliersByCountry(country)` método en el nivel de lógica empresarial para invocar el `GetSuppliers()` método cuando el parámetro de país es `null` y llamar a la DAL `GetSuppliersByCountry(country)` en caso contrario, el método. Esto tendrá el efecto de devolver todos los proveedores cuando no se especifica ningún país y al subconjunto adecuado de proveedores cuando se incluye el parámetro de país.

Cambiar el `GetSuppliersByCountry(country)` método en el `SuppliersBLL` clase a lo siguiente:

[!code-csharp[Main](declarative-parameters-cs/samples/sample4.cs)]

Con este cambio la `DeclarativeParams.aspx` página muestra todos los proveedores cuando visita en primer lugar (o cada vez que la `CountryName` cuadro de texto está vacía).


[![Todos los proveedores son ahora se muestran de forma predeterminada](declarative-parameters-cs/_static/image29.png)](declarative-parameters-cs/_static/image28.png)

**Figura 10**: todos los proveedores son ahora se muestran de forma predeterminada ([haga clic aquí para ver imagen en tamaño completo](declarative-parameters-cs/_static/image30.png))


## <a name="summary"></a>Resumen

Para usar los métodos con parámetros de entrada, necesitamos especificar los valores para los parámetros en el ObjectDataSource `SelectParameters` colección. Los diferentes tipos de parámetros permiten que se obtienen de orígenes diferentes, el valor de parámetro. El tipo de parámetro predeterminado utiliza un valor codificado de forma rígida, pero la manera tan sencilla (y sin una línea de código) se pueden obtener los valores de parámetro en la cadena de consulta, variables sesión, las cookies e incluso escrito por el usuario los valores de los controles en la página Web.

Los ejemplos que analizamos en este tutorial muestran cómo usar los valores de parámetro declarativa. Sin embargo, puede haber ocasiones se deba utilizar un origen de parámetro que no está disponible, como la fecha y hora actuales, o, si usaba el pertenencia, el identificador de usuario del visitante de nuestro sitio. Para tales escenarios, podemos establecer los valores de parámetro mediante programación antes de ObjectDataSource invoca el método de su subyacente del objeto. Veremos cómo hacerlo en el [siguiente tutorial](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-objectdatasource-cs.md)
> [Siguiente](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
