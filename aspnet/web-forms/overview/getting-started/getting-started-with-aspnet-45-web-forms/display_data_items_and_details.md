---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Datos para mostrar los elementos y detalles | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 083f7182416012c85f05db255fcab4d8e535b52a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820352"
---
<a name="display-data-items-and-details"></a>Datos para mostrar los elementos y detalles
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible para acompañar esta serie de tutoriales.


Este tutorial describe cómo mostrar los elementos de datos y los detalles del elemento de datos con formularios Web Forms ASP.NET y Entity Framework Code First. Este tutorial se basa en el tutorial anterior "Interfaz de usuario y navegación" y forma parte de la serie de tutoriales de Wingtip Toys Store. Cuando haya completado este tutorial, podrá ver los productos en el *ProductsList.aspx* página y detalles sobre un producto individual en el *ProductDetails.aspx* página.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo agregar un control de datos para mostrar los productos de la base de datos.
- Cómo conectar un control de datos a los datos seleccionados.
- Cómo agregar un control de datos para mostrar los detalles del producto de la base de datos.
- Cómo recuperar un valor de la cadena de consulta y use ese valor para limitar los datos que se recuperan de la base de datos.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Estas son las características introducidas en el tutorial:

- Enlace de modelos
- Proveedores de valor

## <a name="adding-a-data-control-to-display-products"></a>Agregar un Control de datos para mostrar los productos

Al enlazar datos a un control de servidor, hay algunas opciones diferentes que puede usar. Las opciones más comunes incluyen la adición de un control de origen de datos, agregar código a mano o mediante el enlace de modelo.

### <a name="using-a-data-source-control-to-bind-data"></a>Uso de un Control de origen de datos para enlazar datos

Agrega un control de origen de datos, podrá vincular el control de origen de datos al control que muestra los datos. Este enfoque le permite conectarse mediante declaración los controles de servidor directamente a orígenes de datos, en lugar de utilizar un enfoque de programación.

### <a name="coding-by-hand-to-bind-data"></a>Codificar de manera manual para enlazar datos

Agregar código a mano implica leer el valor, comprobación de un valor null, intenta convertirlo al tipo adecuado, comprobando si la conversión tuvo éxito y por último, mediante el valor de la consulta. Podría usar este enfoque cuando deba conservar el control total sobre la lógica de acceso a datos.

### <a name="using-model-binding-to-bind-data"></a>Uso de enlace para enlazar datos de modelos

Uso de enlace de modelos le permite enlazar los resultados con mucho menos código y le ofrece la capacidad de volver a usar la funcionalidad en toda la aplicación. El enlace de modelos pretende simplifican el trabajo con lógica de acceso a datos centrada en el código mientras se conservan las ventajas de un marco de enlace de datos enriquecido.

## <a name="displaying-products"></a>Visualización de productos

En este tutorial, usará el enlace de modelos para enlazar datos. Para configurar un control de datos para utilizar el enlace de modelos para seleccionar datos, debe establecer el control `SelectMethod` propiedad en el nombre de un método en el código de la página. El control de datos llama al método en el momento adecuado en el ciclo de vida de página y enlaza automáticamente los datos devueltos. No hace falta llamar explícitamente a la `DataBind` método.

Mediante los pasos siguientes, modificará el marcado en el *ProductList.aspx* página para que la página puede mostrar los productos.

1. En **el Explorador de soluciones**, abra el *ProductList.aspx* página.
2. Reemplace el marcado existente por el siguiente marcado:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Este código usa un **ListView** control denominado "Listadeproductos" para mostrar los productos.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

El **ListView** control muestra los datos en un formato que se define mediante plantillas y estilos. Resulta útil para los datos en una estructura de repetición. Esto **ListView** ejemplo simplemente muestran los datos de la base de datos, pero puede habilitar a los usuarios editar, insertar y eliminar datos y ordenar y datos de la página, todo ello sin código.

Estableciendo el `ItemType` propiedad en el **ListView** controlar, la expresión de enlace de datos `Item` está disponible y el control pasa a ser inflexible. Como se mencionó en el tutorial anterior, puede seleccionar los detalles del objeto de elemento con IntelliSense, como especificar el `ProductName`:

![Mostrar datos de elementos y detalles - IntelliSense](display_data_items_and_details/_static/image1.png)

Además, se utiliza un enlace de modelo para especificar un `SelectMethod` valor. Este valor (`GetProducts`) se corresponderá con el método que va a agregar al código subyacente para mostrar los productos en el paso siguiente.

### <a name="adding-code-to-display-products"></a>Agregar código para mostrar los productos

En este paso, agregará código para rellenar el **ListView** control con datos de producto de la base de datos. El código será compatible con la que muestra los productos por categoría individual, así como que muestra todos los productos.

1. En **el Explorador de soluciones**, haga clic en *ProductList.aspx* y, a continuación, haga clic en **ver código**.
2. Reemplace el código existente en el *ProductList.aspx.cs* archivo con el código siguiente:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Este código muestra la `GetProducts` método al que hace referencia el `ItemType` propiedad de la **ListView** en controlar la *ProductList.aspx* página. Para limitar los resultados a una categoría específica en la base de datos, el código establece la `categoryId` valor desde el valor de cadena de consulta pasan a la *ProductList.aspx* página cuando la *ProductList.aspx* es la página navega. El `QueryStringAttribute` clase en el `System.Web.ModelBinding` espacio de nombres se usa para recuperar el valor del identificador de variable de cadena de consulta. Esto indica que el enlace de modelos para intentar enlazar un valor de cadena de consulta que el `categoryId` parámetro en tiempo de ejecución.

Cuando se pasa una categoría válida como una cadena de consulta a la página, los resultados de la consulta se limitan a esos productos en la base de datos que coinciden con la `categoryId` valor. Por ejemplo, si la dirección URL para el *ProductsList.aspx* página es la siguiente:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

La página muestra solo los productos donde la `category` es igual a `1`.

Si no se incluye ninguna cadena de consulta cuando se desplace a la *ProductList.aspx* página, se mostrarán todos los productos.

Los orígenes de los valores de estos métodos se conocen como *proveedores de valor* (como *QueryString*), y se conocen los atributos de parámetro que indican qué proveedor de valor a utilizar como valor los atributos de proveedor (como "`id`"). ASP.NET incluye proveedores de valor y los atributos correspondientes para todos los orígenes de entrada de usuario habituales en una aplicación de formularios Web Forms, como la cadena de consulta, cookies, valores de formulario, controles, estado de vista, el estado de sesión y las propiedades de perfil. También puede escribir proveedores de valores personalizados.

### <a name="running-the-application"></a>Ejecutar la aplicación

Ejecute ahora la aplicación para ver cómo puede ver todos los productos o solo un conjunto de productos limitado por categoría.

1. En el **el Explorador de soluciones**, haga clic en el *Default.aspx* página y seleccione **ver en el explorador**.  
 El explorador se abrirá y mostrará el *Default.aspx* página.
2. Seleccione **automóviles** desde el menú de navegación de la categoría de producto.  
 El *ProductList.aspx* se muestra la página que muestra solo los productos incluidos en la categoría "Automóviles". Más adelante en este tutorial, mostrará los detalles del producto.  

    ![Mostrar datos de elementos y detalles - automóviles](display_data_items_and_details/_static/image2.png)
3. Seleccione **productos** en el menú de navegación en la parte superior.  
 Nuevamente, el *ProductList.aspx* se muestra la página, pero esta vez muestra toda la lista de productos.   

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image3.png)
4. Cierre el explorador y vuelva a Visual Studio.

### <a name="adding-a-data-control-to-display-product-details"></a>Agregar un Control de datos para mostrar los detalles del producto

A continuación, modificará el marcado en el *ProductDetails.aspx* página que agregó en el tutorial anterior para que la página puede mostrar información sobre un producto individual.

1. En **el Explorador de soluciones**, abra el *ProductDetails.aspx* página.
2. Reemplace el marcado existente por el siguiente marcado:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Este código usa un **FormView** control para mostrar los detalles sobre un producto individual. Este marcado usa métodos, como los que se usan para mostrar datos en el *ProductList.aspx* página. El **FormView** control se usa para mostrar un único registro a la vez desde un origen de datos. Cuando se usa el **FormView** control, crea plantillas para mostrar y editar valores enlazados a datos. Las plantillas contienen controles, expresiones de enlace y el formato que definen la apariencia y funcionalidad del formulario.

Para conectar el marcado anterior a la base de datos, debe agregar código adicional para la *ProductDetails.aspx* código.

1. En **el Explorador de soluciones**, haga clic en *ProductDetails.aspx* y, a continuación, haga clic en **ver código**.  
   El *ProductDetails.aspx.cs* se mostrará el archivo.
2. Reemplace el código existente con el siguiente código:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Este código comprueba si hay un "`productID`" valor de cadena de consulta. Si se encuentra un valor de cadena de consulta válida, se muestra el producto coincidente. Si no se encuentra ninguna cadena de consulta o el valor de cadena de consulta no es válido, no se muestra ningún producto en el *ProductDetails.aspx* página.

### <a name="running-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para ver un producto individual que se muestra según el identificador del producto.

1. Presione **F5** mientras se encuentre en Visual Studio para ejecutar la aplicación.  
 El explorador se abrirá y mostrará el *Default.aspx* página.
2. Seleccione "Barcos" en el menú de navegación de la categoría.  
 El *ProductList.aspx* se muestra la página.
3. Seleccione el producto "Papel barco" de la lista de productos.  
 El *ProductDetails.aspx* se muestra la página.   

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image4.png)
4. Cierre el explorador.

## <a name="summary"></a>Resumen

En este tutorial de la serie agregar marcado y código para mostrar una lista de productos y mostrar los detalles del producto. Durante este proceso ha aprendido acerca de los controles de datos fuertemente tipados, enlace de modelos y los proveedores de valor. En el siguiente tutorial, agregará un carro de la compra a la aplicación de ejemplo Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionales

[Recuperar y mostrar datos con enlace de modelos y formularios web forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Anterior](ui_and_navigation.md)
> [Siguiente](shopping-cart.md)
