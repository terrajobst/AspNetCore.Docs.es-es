---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Establecer mediante programación los valores de parámetro de ObjectDataSource (VB) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial se examinará agregar un método a nuestro DAL y BLL que acepta un parámetro de entrada y devuelve los datos. En el ejemplo se establecerá este parámetro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: ac53d651601829b6e7d2ce312a084618a8afbb61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876675"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Establecer mediante programación los valores de parámetro de ObjectDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) o [descarga de PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> En este tutorial se examinará agregar un método a nuestro DAL y BLL que acepta un parámetro de entrada y devuelve los datos. En el ejemplo se establecerá este parámetro mediante programación.


## <a name="introduction"></a>Introducción

Como vimos en el [tutorial anterior](declarative-parameters-vb.md), una serie de opciones está disponible para pasar mediante declaración los valores de parámetros a métodos de ObjectDataSource. Si el valor del parámetro está codificado de forma rígida, proviene de un control Web en la página, o en cualquier otro origen que es legible por un origen de datos `Parameter` objeto, por ejemplo, que se puede enlazar valor al parámetro de entrada sin tener que escribir una línea de código.

Puede haber ocasiones, sin embargo, cuando el valor del parámetro procede de algún origen ya no tienen en cuenta uno del origen de datos integrados `Parameter` objetos. Si nuestro sitio admite cuentas de usuario que queramos establezca el parámetro tomando como base el actualmente ha iniciado sesión Id. de usuario del visitante O bien, podemos necesitar personalizar el valor del parámetro antes de enviarlo a lo largo de al método del objeto subyacente de ObjectDataSource.

Cada vez que el ObjectDataSource `Select` se invoca el método genera el ObjectDataSource en primer lugar su [evento Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). A continuación, se invoca el método del objeto subyacente de ObjectDataSource. Una vez que se haya completado la ObjectDataSource [eventos seleccionados](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) se activa (figura 1 muestra esta secuencia de eventos). Pueden establecer los valores de parámetro pasados al método del objeto subyacente de ObjectDataSource o personalizados en un controlador de eventos para el `Selecting` eventos.


[![Se invoca el ObjectDataSource seleccionados y método de selección de incendio de eventos antes y después de su subyacente del objeto](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figura 1**: The ObjectDataSource `Selected` y `Selecting` eventos activan antes y del después de su objeto subyacente método se invoca ([haga clic aquí para ver la imagen a tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


En este tutorial se examinará agregar un método a nuestro DAL y BLL que acepta un parámetro de entrada `Month`, del tipo `Integer` y devuelve un `EmployeesDataTable` objeto rellenado con los empleados que tienen su aniversario de contratación de la manera especificada `Month`. Nuestro ejemplo establecerá este parámetro mediante programación en función del mes actual, que muestra una lista de "Employee aniversarios este mes".

Comencemos.

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Paso 1: Agregar un método a`EmployeesTableAdapter`

En nuestro ejemplo primero tenemos que agregar a fin de recuperar los empleados cuyo `HireDate` se produjo en un mes especificado. Para proporcionar esta funcionalidad de acuerdo con nuestra arquitectura se debe crear un método en `EmployeesTableAdapter` que se asigna a la instrucción SQL apropiada. Para ello, inicie abriendo el conjunto de datos con tipo de Northwind. Haga doble clic en la `EmployeesTableAdapter` etiqueta y elija Agregar consulta.


[![Agregar una nueva consulta a la EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figura 2**: agregar una nueva consulta para la `EmployeesTableAdapter` ([haga clic aquí para ver la imagen a tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Elija esta opción para agregar una instrucción SQL que devuelve filas. Cuando llegue a especificar una `SELECT` el valor predeterminado de la pantalla de instrucción `SELECT` instrucción para el `EmployeesTableAdapter` ya se va a cargar. Basta con agregar en el `WHERE` cláusula: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) es una función de T-SQL que devuelve una parte de fecha en particular de un `datetime` tipo; en este caso usamos `DATEPART` para devolver el mes de la `HireDate` columna.


[![Solo las filas donde el HireDate columna de retorno es menor o igual que el @HiredBeforeDate parámetro](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figura 3**: devolver sólo aquellas filas donde la `HireDate` columna es menor o igual que el `@HiredBeforeDate` parámetro ([haga clic aquí para ver la imagen a tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Por último, cambie el `FillBy` y `GetDataBy` a los nombres de método `FillByHiredDateMonth` y `GetEmployeesByHiredDateMonth`, respectivamente.


[![Elija los nombres de método más adecuados que FillBy y GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Figura 4**: elija más adecuado método nombres que `FillBy` y `GetDataBy` ([haga clic aquí para ver la imagen a tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Haga clic en Finalizar para completar al asistente y volver a la superficie de diseño del conjunto de datos. El `EmployeesTableAdapter` ahora se debe incluir un nuevo conjunto de métodos para tener acceso a empleados contratados en un mes especificado.


[![Los nuevos métodos aparecen en la superficie de diseño del conjunto de datos](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figura 5**: los nuevos métodos aparecen en la superficie de diseño del conjunto de datos ([haga clic aquí para ver la imagen a tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Paso 2: Agregar el`GetEmployeesByHiredDateMonth(month)`método a la capa de lógica de negocios

Puesto que nuestra aplicación arquitectura usa un independiente de las capas para la lógica de negocios y los datos tener acceso a lógica, es necesario agregar un método a nuestro BLL que llama a la capa DAL para recuperar a los empleados contratado antes de una fecha especificada. Abra la `EmployeesBLL.vb` de archivos y agregue el método siguiente:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Al igual que con nuestros otros métodos de esta clase, `GetEmployeesByHiredDateMonth(month)` simplemente llama hacia abajo en la capa DAL y devuelve los resultados.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Paso 3: Mostrar a los empleados cuyo aniversario contratación es este mes

El último paso en este ejemplo es mostrar a los empleados cuyo aniversario contratación es este mes. Empiece agregando un control GridView a la `ProgrammaticParams.aspx` página en el `BasicReporting` carpeta y agregue un nuevo ObjectDataSource como origen de datos. Configurar el ObjectDataSource para usar el `EmployeesBLL` clase con la `SelectMethod` establecido en `GetEmployeesByHiredDateMonth(month)`.


[![Utilice la clase EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figura 6**: Use la `EmployeesBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Seleccione desde el GetEmployeesByHiredDateMonth(month) (método)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figura 7**: Select From el `GetEmployeesByHiredDateMonth(month)` método ([haga clic aquí para ver la imagen a tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


La última pantalla le pide que proporcione el `month` origen del valor del parámetro. Puesto que este valor se configuran mediante programación, deje el origen del parámetro establece en el valor predeterminado None opción y haga clic en Finalizar.


[![Deje el origen de parámetro establecido en None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figura 8**: deje el origen de parámetro establecido en None ([haga clic aquí para ver la imagen a tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Esto creará un `Parameter` objeto en el ObjectDataSource `SelectParameters` colección que no tiene un valor especificado.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Para establecer este valor mediante programación, es necesario crear un controlador de eventos para el ObjectDataSource `Selecting` eventos. Para ello, vaya a la vista de diseño y haga doble clic en el origen ObjectDataSource. Como alternativa, seleccione el ObjectDataSource, vaya a la ventana Propiedades y haga clic en el icono de rayo. A continuación, en haga doble clic en el cuadro de texto junto a la `Selecting` eventos o escriba el nombre del controlador de eventos que desea usar. Como una tercera opción, puede crear el controlador de eventos seleccionando el ObjectDataSource y su `Selecting` eventos en las dos listas de lista desplegable en la parte superior de la clase de código subyacente de la página.


![Haga clic en el icono de rayo en la ventana Propiedades para mostrar eventos de un Control Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figura 9**: haga clic en el icono de rayo en la ventana Propiedades para mostrar eventos de un Control Web


Los tres métodos agregar un nuevo controlador de eventos para el ObjectDataSource `Selecting` eventos a la clase de código subyacente de la página. En este controlador de eventos podemos leer y escribir en los valores de parámetro mediante `e.InputParameters(parameterName)`, donde *`parameterName`* es el valor de la `Name` de atributo en el `<asp:Parameter>` etiqueta (el `InputParameters` colección también puede ser indizada ordinal, como en `e.InputParameters(index)`). Para establecer el `month` parámetro en el mes actual, agregue lo siguiente a la `Selecting` controlador de eventos:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Cuando visite esta página a través de un explorador podemos ver que el solo empleado se contrató a este mes (marzo) Laura Callahan, que está en la compañía desde 1994.


[![Los empleados cuyos aniversarios se muestran en este mes](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figura 10**: los empleados cuyo aniversarios este mes se muestran ([haga clic aquí para ver la imagen a tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Resumen

Mientras que los valores de parámetros de ObjectDataSource normalmente se establece mediante declaración, sin necesidad de una línea de código, es fácil establecer los valores de parámetro mediante programación. Todo lo que necesitamos hacer es crear un controlador de eventos para el ObjectDataSource `Selecting` evento, que se desencadena antes de método del objeto subyacente se invoca y establecer manualmente los valores para uno o más parámetros a través de la `InputParameters` colección.

Este tutorial finaliza la sección informes básicos. El [siguiente tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) inicia, la sección de filtrado y escenarios de maestra / de detalle, en el que se examinará técnicas para permitir que el visitante para filtrar los datos y los detalles de un informe principal en un informe de detalles.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Hilton Giesenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](declarative-parameters-vb.md)
