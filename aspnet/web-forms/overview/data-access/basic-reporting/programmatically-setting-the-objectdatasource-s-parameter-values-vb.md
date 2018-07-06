---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Establecer mediante programación los valores de parámetro de ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, echaremos un vistazo a la adición de un método a nuestro DAL y BLL que acepta un parámetro de entrada y devuelve los datos. En el ejemplo se establecerá este parámetro...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: d779de4f5bd0d03f413237689e5a64330fcb491d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825787"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Establecer mediante programación los valores de parámetro de ObjectDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) o [descargar PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> En este tutorial, echaremos un vistazo a la adición de un método a nuestro DAL y BLL que acepta un parámetro de entrada y devuelve los datos. En el ejemplo se establecerá este parámetro mediante programación.


## <a name="introduction"></a>Introducción

Como hemos visto en el [tutorial anterior](declarative-parameters-vb.md), una serie de opciones está disponible para pasar de forma declarativa los valores de parámetro a los métodos de ObjectDataSource. Si el valor del parámetro está codificado de forma rígida, procede de un control Web en la página o en cualquier otro origen que es legible por un origen de datos `Parameter` objeto, por ejemplo, que se puede enlazar valor al parámetro de entrada sin tener que escribir una línea de código.

Puede haber ocasiones, sin embargo, cuando el valor del parámetro se trata de algún origen ya no tienen en cuenta uno del origen de datos integrados `Parameter` objetos. Si nuestro sitio admite las cuentas de usuario que podría desear establecer el parámetro en función de la sesión iniciada actualmente ID de usuario. del visitante O podemos necesitar personalizar el valor del parámetro antes de enviarlo a lo largo de método del objeto subyacente de ObjectDataSource.

Cada vez que el ObjectDataSource `Select` se invoca el método genera ObjectDataSource en primer lugar su [evento Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). A continuación, se invoca el método del objeto subyacente de ObjectDataSource. Una vez que se haya completado el ObjectDataSource [eventos seleccionados](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) se activa (figura 1 muestra esta secuencia de eventos). Se pueden establecer los valores de parámetro que se pasó al método del objeto subyacente de ObjectDataSource o personalizados en un controlador de eventos para el `Selecting` eventos.


[![Se invoca seleccionados y el método de selección de fuego de eventos antes y después de su subyacente del objeto de ObjectDataSource](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figura 1**: el origen ObjectDataSource `Selected` y `Selecting` eventos Fire antes y del después de su objeto subyacente método se invoca ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


En este tutorial, echaremos un vistazo a la adición de un método a nuestro DAL y BLL que acepta un parámetro de entrada `Month`, del tipo `Integer` y devuelve un `EmployeesDataTable` objeto rellenado con los empleados que tienen su aniversario contratación especificado `Month`. En nuestro ejemplo establecerá este parámetro mediante programación en función del mes actual, que muestra una lista de "Employee aniversarios este mes".

Comencemos.

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Paso 1: Agregar un método a`EmployeesTableAdapter`

En nuestro ejemplo primero que necesitamos agregar un medio para recuperar aquellos empleados cuyo `HireDate` se produjo en un mes especificado. Para proporcionar esta funcionalidad de acuerdo con nuestra arquitectura se debe crear primero un método en `EmployeesTableAdapter` que se asigna a la instrucción SQL adecuada. Para lograr esto, comience abriendo el conjunto de datos de Northwind con tipo. Haga doble clic en el `EmployeesTableAdapter` etiquetar y seleccionar Add Query.


[![Agregar una nueva consulta a la EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figura 2**: agregar una nueva consulta a la `EmployeesTableAdapter` ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Elija esta opción para agregar una instrucción SQL que devuelve filas. Cuando llegue a la especificación de un `SELECT` el valor predeterminado de la pantalla de instrucción `SELECT` instrucción para el `EmployeesTableAdapter` ya se va a cargar. Basta con agregar en el `WHERE` cláusula: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) es una función de Transact-SQL que devuelve una parte de fecha en particular de un `datetime` tipo; en este caso usamos `DATEPART` para devolver el mes de la `HireDate` columna.


[![Solo las filas donde la HireDate columna return es menor o igual que el @HiredBeforeDate parámetro](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figura 3**: devolver solo las filas donde la `HireDate` columna es menor o igual que el `@HiredBeforeDate` parámetro ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Por último, cambie el `FillBy` y `GetDataBy` los nombres de método a `FillByHiredDateMonth` y `GetEmployeesByHiredDateMonth`, respectivamente.


[![Elija los nombres de método más adecuados que FillBy y GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Figura 4**: elija más adecuado método nombres que `FillBy` y `GetDataBy` ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Haga clic en Finalizar para completar al asistente y volver a la superficie de diseño del conjunto de datos. El `EmployeesTableAdapter` debe incluir ahora un nuevo conjunto de métodos para tener acceso a los empleados contratados en un mes especificado.


[![Los nuevos métodos aparecen en la superficie de diseño del conjunto de datos](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figura 5**: el nuevo métodos aparecen en la superficie de diseño del conjunto de datos ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Paso 2: Agregar el`GetEmployeesByHiredDateMonth(month)`método a la capa de lógica de negocios

Dado que nuestra aplicación arquitectura usa otra capa para la lógica de negocios y datos de acceso a la lógica, necesitamos agregar un método a nuestro BLL que las llamadas a la capa DAL para recuperar a los empleados contratadas antes que una fecha especificada. Abra el `EmployeesBLL.vb` archivo y agregue el siguiente método:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Al igual que con los otros métodos de esta clase, `GetEmployeesByHiredDateMonth(month)` simplemente llama a hacia abajo en la capa DAL y devuelve los resultados.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Paso 3: Mostrar a los empleados cuyo aniversario de contratación es este mes

El último paso en este ejemplo es mostrar a los empleados cuyo aniversario de contratación es este mes. Empiece agregando un control GridView a la `ProgrammaticParams.aspx` página en el `BasicReporting` carpeta y agregue un nuevo origen ObjectDataSource como origen de datos. Configurar el origen ObjectDataSource para usar el `EmployeesBLL` clase con el `SelectMethod` establecido en `GetEmployeesByHiredDateMonth(month)`.


[![Utilice la clase EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figura 6**: Use el `EmployeesBLL` clase ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Seleccione desde el GetEmployeesByHiredDateMonth(month) (método)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figura 7**: Select From el `GetEmployeesByHiredDateMonth(month)` método ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


La pantalla final nos pide que proporcione el `month` origen del valor del parámetro. Puesto que este valor también se establece mediante programación, deje el origen de los parámetros establecidos en el valor predeterminado None opción y haga clic en Finalizar.


[![Deje el origen de parámetro establecido en None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figura 8**: deje el código fuente establecido en None ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Esto creará un `Parameter` objeto en el ObjectDataSource `SelectParameters` colección que no tiene un valor especificado.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Para establecer este valor mediante programación, es necesario crear un controlador de eventos para el origen de ObjectDataSource `Selecting` eventos. Para ello, vaya a la vista de diseño y haga doble clic en el origen ObjectDataSource. Como alternativa, seleccione el origen ObjectDataSource, vaya a la ventana Propiedades y haga clic en el icono de rayo. A continuación, ya sea haga doble clic en el cuadro de texto junto a la `Selecting` eventos o escriba el nombre, el controlador de eventos que desea usar. Como alternativa, puede crear el controlador de eventos seleccionando ObjectDataSource y su `Selecting` evento desde las dos listas desplegables en la parte superior de la clase de código subyacente de la página.


![Haga clic en el icono de rayo en la ventana Propiedades para mostrar los eventos de un Control Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figura 9**: haga clic en el icono de rayo en la ventana Propiedades para mostrar los eventos de un Control Web


Los tres enfoques agregar un nuevo controlador de eventos para el origen de ObjectDataSource `Selecting` eventos a la clase de código subyacente de la página. En este controlador de eventos podemos leer y escribir en los valores de parámetro mediante `e.InputParameters(parameterName)`, donde *`parameterName`* es el valor de la `Name` atributo en el `<asp:Parameter>` etiqueta (el `InputParameters` colección también puede ser indizada ordinal, como en `e.InputParameters(index)`). Para establecer el `month` parámetro en el mes actual, agregue lo siguiente a la `Selecting` controlador de eventos:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Al visitar esta página a través de un explorador podemos ver que sólo un empleado ha sido contratado este mes (marzo) Laura Callahan, quién ha trabajado con la empresa desde 1994.


[![Los empleados cuyos aniversarios se muestran en este mes](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figura 10**: los empleados cuyo aniversarios este mes se muestran ([haga clic aquí para ver imagen en tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Resumen

Mientras que los valores de parámetros de ObjectDataSource normalmente pueden establecerse de forma declarativa, sin necesidad de una línea de código, es fácil establecer los valores de parámetro mediante programación. Todo lo que necesitamos hacer es crear un controlador de eventos para el origen de ObjectDataSource `Selecting` eventos, que se desencadena antes de método del objeto subyacente se invoca y establecer manualmente los valores para uno o varios parámetros a través de la `InputParameters` colección.

Este tutorial finaliza la sección informes básicos. El [siguiente tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) a partir de la sección de filtrado y escenarios de detalles principales, en el que se examinará técnicas para permitir que el visitante para filtrar los datos y explorar en profundidad desde un informe principal en un informe de detalles.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](declarative-parameters-vb.md)
