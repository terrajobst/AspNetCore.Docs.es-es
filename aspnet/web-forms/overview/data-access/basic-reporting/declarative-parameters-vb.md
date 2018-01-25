---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: "Parámetros declarativos (VB) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial, mostraremos cómo usar un parámetro establecido en un valor codificado de forma rígida para seleccionar los datos que se va a mostrar en un control DetailsView."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: ea1aed2b76eb4196196f8a800c0bdb891bceda91
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="declarative-parameters-vb"></a>Parámetros declarativos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) o [descarga de PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> En este tutorial, mostraremos cómo usar un parámetro establecido en un valor codificado de forma rígida para seleccionar los datos que se va a mostrar en un control DetailsView.


## <a name="introduction"></a>Introducción

En el [último tutorial](displaying-data-with-the-objectdatasource-vb.md) analizamos mostrar datos con los controles GridView, DetailsView y FormView enlazados a un control ObjectDataSource que invoca la `GetProducts()` método desde el `ProductsBLL` clase. El `GetProducts()` método devuelve una tabla de datos fuertemente tipados rellenado con todos los registros de la base de datos de Northwind `Products` tabla. El `ProductsBLL` clase contiene métodos adicionales para devolver subconjuntos solo de los productos - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, y `GetProductsBySupplierID(supplierID)`. Estos tres métodos esperan un parámetro de entrada que indica cómo filtrar la información de producto devuelto.

ObjectDataSource puede usarse para invocar métodos que esperan parámetros de entrada, pero para ello se debe especificar dónde provienen los valores para estos parámetros. Los valores de parámetro puede estar codificado de forma rígida o pueden proceder de una variedad de orígenes dinámicos, incluidos: valores de cadena de consulta, variables de sesión, el valor de propiedad de un control Web en la página o a otras personas.

Para este tutorial Empecemos por que ilustra cómo usar un parámetro establecido en un valor codificado de forma rígida. En concreto, veremos cómo agregar un DetailsView a la página que muestra información sobre un producto concreto, es decir, del Chef Anton tártara, que tiene un `ProductID` de 5. A continuación, veremos cómo establecer el valor del parámetro basado en un control Web. En concreto, vamos a usar un cuadro de texto para permitir que el usuario escriba en un país o región, después del cual pueden hacer clic en un botón para ver la lista de proveedores que residen en ese país.

## <a name="using-a-hard-coded-parameter-value"></a>Uso de un valor de parámetro codificado de forma rígida

Para el primer ejemplo, empiece por agregar un control DetailsView a la `DeclarativeParams.aspx` página en el `BasicReporting` carpeta. En las etiquetas inteligentes de DetailsView, seleccione &lt;nuevo origen de datos&gt; en la lista desplegable lista y optar por agregar un ObjectDataSource.


[![Agregue un ObjectDataSource a la página](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Figura 1**: agregar un ObjectDataSource a la página ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image3.png))


Se iniciará automáticamente el Asistente de Elegir origen de datos del control ObjectDataSource. Seleccione el `ProductsBLL` clase a partir de la primera pantalla del asistente.


[![Seleccione la clase ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Figura 2**: seleccione la `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image6.png))


Puesto que deseamos mostrar información sobre un producto determinado que deseamos utilizar la `GetProductByProductID(productID)` método.


[![Elija el método GetProductByProductID(productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Figura 3**: elija la `GetProductByProductID(productID)` método ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image9.png))


Dado que el método seleccionados incluye un parámetro, hay una pantalla más para que el asistente, donde se nos solicita para definir el valor que se utilizará para el parámetro. La lista de la izquierda muestra todos los parámetros para el método seleccionado. Para `GetProductByProductID(productID)` solo hay un `productID`. En la parte derecha podemos especificamos el valor para el parámetro seleccionado. La lista desplegable de origen de parámetro enumera los diversos orígenes posibles para el valor del parámetro. Puesto que deseamos especificar un valor codificado de forma rígida de 5 para el `productID` parámetro, deje el origen del parámetro como Ninguno y 5 en el cuadro de texto DefaultValue.


[![Un Hard-Coded parámetro valor de 5 se utilizará para el parámetro productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Figura 4**: A Hard-Coded parámetro valor de 5 se utilizará para la `productID` parámetro ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image12.png))


Después de completar el Asistente para configurar orígenes de datos, que incluye marcado declarativo del control ObjectDataSource un `Parameter` objeto en el `SelectParameters` recopilación para cada uno de los parámetros de entrada esperados por el método definido en el `SelectMethod` propiedad. Puesto que el método usamos en este ejemplo espera sólo un único parámetro de entrada, `parameterID`, hay sólo una entrada aquí. El `SelectParameters` colección puede contener cualquier clase que deriva de la `Parameter` clase en el `System.Web.UI.WebControls` espacio de nombres. Para la base de los valores de parámetro codificado de forma rígida `Parameter` se utiliza la clase, pero el otro parámetro de origen de opciones de una derivada `Parameter` se utiliza la clase; también puede crear sus propias [tipos de parámetros personalizados](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), si es necesario.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Si va a lo largo de su propio equipo el marcado declarativo que se ve en este momento puede incluir valores de la `InsertMethod`, `UpdateMethod`, y `DeleteMethod` propiedades, así como `DeleteParameters`. Asistente para elegir origen de datos de ObjectDataSource especifica automáticamente los métodos de la `ProductBLL` que se utilizará para insertar, actualizar y eliminar, por lo que, a menos que explícitamente desactivada dichas decisiones más, deberá incluirse en el marcado.


Cuando visite esta página, los datos de control Web invocará el ObjectDataSource `Select` método, que llamará el `ProductsBLL` la clase `GetProductByProductID(productID)` método utilizando el valor codificado de forma rígida de 5 para el `productID` parámetro de entrada. El método devolverá fuertemente tipadas `ProductDataTable` objeto que contiene una sola fila con información acerca tártara del Chef Anton (el producto con `ProductID` 5).


[![Se muestra tártara información acerca del Chef Anton](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Figura 5**: se muestran tártara información acerca del Chef Anton ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Establecer el valor del parámetro en el valor de propiedad de un Control Web

Parámetro de ObjectDataSource valores también se pueden establecer en función del valor de un control Web en la página. Para ilustrar esto, supongamos que tiene un control GridView que muestra todos los proveedores que se encuentran en un país o región especificado por el usuario. Para realizar esta guía de inicio mediante la adición de un cuadro de texto a la página en la que el usuario puede escribir un nombre de país. Establecer este control de cuadro de texto `ID` propiedad `CountryName`. Agregar un control Web Button.


[![Agregue un cuadro de texto a la página con el Id. de CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Figura 6**: agregar un cuadro de texto a la página con `ID` `CountryName` ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image18.png))


A continuación, agregar un control GridView a la página y, de la etiqueta inteligente, optar por agregar una nueva ObjectDataSource. Puesto que deseamos mostrar seleccionar información de proveedor la `SuppliersBLL` clase a partir de la pantalla del asistente primera. En la segunda pantalla, seleccione la `GetSuppliersByCountry(country)` método.


[![Elija el método GetSuppliersByCountry(country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Figura 7**: elija la `GetSuppliersByCountry(country)` método ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image21.png))


Puesto que la `GetSuppliersByCountry(country)` método tiene un parámetro de entrada, una vez más, el asistente incluye una pantalla final para elegir el valor del parámetro. Esta vez, establezca el parámetro source al Control. Esto rellenará la lista desplegable de ControlID con los nombres de los controles en la página; Seleccione el `CountryName` control de la lista. Cuando primero se visita la página de la `CountryName` cuadro de texto se quedan en blanco, por lo que se devuelve ningún resultado y no se muestra nada. Si desea mostrar los resultados de forma predeterminada, establezca el cuadro de texto DefaultValue en consecuencia.


[![Establezca el valor de parámetro en el valor del Control CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Figura 8**: establece el valor del parámetro en el `CountryName` valor de Control ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image24.png))


Marcado declarativo del ObjectDataSource difiere ligeramente de nuestro primer ejemplo, con un [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) en lugar de la norma `Parameter` objeto. A `ControlParameter` tiene propiedades adicionales para especificar el `ID` de control de Web y el valor de propiedad que se utilizará para el parámetro (`PropertyName`). El Asistente para configurar orígenes de datos fue lo suficientemente inteligente como para determinar que, para un cuadro de texto, probablemente querrá usar la `Text` propiedad para el valor del parámetro. Si, sin embargo, desea utilizar un valor de otra propiedad de control Web puede cambiar la `PropertyName` valor aquí o haciendo clic en el vínculo "Mostrar propiedades avanzadas" en el asistente.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Al visitar la página por primera vez el `CountryName` cuadro de texto está vacío. El ObjectDataSource `Select` todavía se invoca el método GridView, pero un valor de `Nothing` se pasa a la `GetSuppliersByCountry(country)` método. Convierte el objeto TableAdapter el `Nothing` en una base de datos `NULL` valor (`DBNull.Value`), pero la consulta usada por la `GetSuppliersByCountry(country)` método se escribe de forma que no devuelva cualquiera valores cuando un `NULL` se especifica el valor para el `@CategoryID`parámetro. En resumen, no se devuelven proveedores.

Una vez que el visitante entra en un país o región, sin embargo y hace clic en el botón Mostrar proveedores para que se produzca una devolución de datos, el ObjectDataSource `Select` método se realiza una nueva consulta, pasando el control de cuadro de texto `Text` valor como el `country` parámetro.


[![Se muestran los proveedores de Canadá](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Figura 9**: se muestran los proveedores de Canadá ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Mostrar todos los proveedores de forma predeterminada

En su lugar de mostrar: ninguno de los proveedores cuando se visualizan en primer lugar la página que podríamos desear mostrar *todos los* proveedores al principio, que permite al usuario reducir la lista, escriba un nombre de país en el cuadro de texto. Cuando el cuadro de texto está vacío, el `SuppliersBLL` la clase `GetSuppliersByCountry(country)` método se pasa en `Nothing` para su  *`country`*  parámetro de entrada. Esto `Nothing` valor, a continuación, se pasa hacia abajo en la capa de DAL `GetSupplierByCountry(country)` método, donde se traduce a una base de datos `NULL` valor para el `@Country` parámetro en la consulta siguiente:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

La expresión `Country = NULL` siempre devuelve False, incluso para los registros cuyo `Country` columna tiene un `NULL` valor; por lo tanto, se devuelve ningún registro.

Para devolver *todos los* proveedores cuando el país en el cuadro de texto está vacío, se puede aumentar la `GetSuppliersByCountry(country)` método en la capa BLL para invocar la `GetSuppliers()` método cuando el parámetro de país es `Nothing` y llamar a la capa de DAL `GetSuppliersByCountry(country)` método en caso contrario. Esto tendrá el efecto de devolver todos los proveedores cuando no se especifica ningún país y el subconjunto apropiado de proveedores cuando se incluye el parámetro de país.

Cambiar el `GetSuppliersByCountry(country)` método en la `SuppliersBLL` clase a lo siguiente:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Con este cambio la `DeclarativeParams.aspx` página muestra todos los proveedores cuando visita por primera vez (o cada vez que la `CountryName` cuadro de texto está vacío).


[![Todos los proveedores son ahora se muestran de forma predeterminada](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Figura 10**: todos los proveedores son ahora se muestran de forma predeterminada ([haga clic aquí para ver la imagen a tamaño completo](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Resumen

Para usar métodos con parámetros de entrada, es preciso especificar los valores para los parámetros en el ObjectDataSource `SelectParameters` colección. Permiten diferentes tipos de parámetros para que el valor del parámetro obtenerse a partir de orígenes diferentes. El tipo de parámetro predeterminado usa un valor codificado de forma rígida, pero al igual que fácilmente (y sin una línea de código) pueden obtenerse valores de parámetro de la querystring, variables de sesión, cookies y valores incluso escrito por el usuario de controles Web de la página.

Los ejemplos que analizamos en este tutorial muestra cómo utilizar los valores de parámetro declarativa. Sin embargo, puede haber veces cuando es necesario utilizar un origen de parámetro que no está disponible, por ejemplo, la fecha y hora actuales, o, si nuestro sitio utilizaba pertenencia, el identificador de usuario del visitante. Para tales escenarios, podemos establecer los valores de parámetro mediante programación antes de ObjectDataSource invocar el método de su objeto subyacente. Veremos cómo hacerlo en el [siguiente tutorial](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Hilton Giesenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](displaying-data-with-the-objectdatasource-vb.md)
[Siguiente](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
