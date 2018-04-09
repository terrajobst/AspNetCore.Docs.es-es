---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Muestre los datos de elementos y le proporcionará detalles | Documentos de Microsoft
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 5fea654aa5116193cb7496c1b9020ed8e25fc06f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="display-data-items-and-details"></a>Muestre los datos de elementos y detalles
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar libros electrónicos (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible como acompañamiento de esta serie de tutoriales.


Este tutorial describe cómo mostrar los elementos de datos y detalles de elemento de datos mediante ASP.NET Web Forms y Entity Framework Code First. Este tutorial se basa en el tutorial anterior "Interfaz de usuario y la navegación" y forma parte de la serie de tutoriales de almacén de Wingtip Toys. Cuando haya completado este tutorial, podrá ver los productos en la *ProductsList.aspx* página y detalles sobre un producto individual en el *ProductDetails.aspx* página.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo agregar un control de datos para mostrar los productos de la base de datos.
- Cómo conectar un control de datos a los datos seleccionados.
- Cómo agregar un control de datos para mostrar detalles de producto de la base de datos.
- Cómo recuperar un valor de la cadena de consulta y use ese valor para limitar los datos que se recuperan de la base de datos.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Estas son las características introducidas en el tutorial:

- Enlace de modelos
- Proveedores de valores

## <a name="adding-a-data-control-to-display-products"></a>Agregar un Control de datos para mostrar los productos

Cuando se enlazan datos a un control de servidor, hay varias opciones que puede usar. Las opciones más comunes incluyen agregar un control de origen de datos, agregar código a mano o mediante el enlace de modelo.

### <a name="using-a-data-source-control-to-bind-data"></a>Uso de un Control de origen de datos para enlazar datos

Agregar un control de origen de datos permite vincular el control de origen de datos al control que muestra los datos. Este enfoque le permite conectarse mediante declaración controles de servidor directamente a orígenes de datos, en lugar de utilizar un enfoque de programación.

### <a name="coding-by-hand-to-bind-data"></a>Tarea de codificación a mano para enlazar datos

Agregar código a mano implica leer el valor, comprobación de un valor null, cualquier intento de convertir al tipo adecuado, comprobando si la conversión tuvo éxito y por último, con el valor en la consulta. Utilice este enfoque cuando es necesario retener un control total sobre la lógica de acceso a datos.

### <a name="using-model-binding-to-bind-data"></a>Uso de enlace para enlazar los datos de modelos

Mediante el enlace de modelo le permite enlazar resultados con mucho menos código y ofrece la posibilidad de volver a usar la funcionalidad a lo largo de la aplicación. Enlace de modelo tiene como objetivo simplifican el trabajo con la lógica de acceso a datos centrada en el código mientras se conservan las ventajas de un marco completo y enlace de datos.

## <a name="displaying-products"></a>Mostrar productos

En este tutorial, usará el enlace de modelos para enlazar los datos. Para configurar un control de datos para utilizar el enlace de modelos para seleccionar datos, debe establecer el control `SelectMethod` propiedad en el nombre de un método en código de la página. El control de datos llama al método en el momento adecuado en el ciclo de vida de la página y enlaza automáticamente los datos devueltos. No es necesario llamar explícitamente a la `DataBind` método.

Con los siguientes pasos, modificará el marcado en el *ProductList.aspx* página para que la página pueda mostrar productos.

1. En **el Explorador de soluciones**, abra el *ProductList.aspx* página.
2. Reemplace el marcado existente con el marcado siguiente:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Este código usa un **ListView** control denominado "Listadeproductos" para mostrar los productos.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

El **ListView** control muestra los datos en un formato que define mediante el uso de plantillas y estilos. Es útil para los datos en una estructura de repetición. Esto **ListView** simplemente ilustra los datos de la base de datos, sin embargo se pueden permitir que los usuarios que va a modificar, insertar y eliminar datos como ordenar y datos de la página, todo ello sin código.

Estableciendo la `ItemType` propiedad en el **ListView** controlar, la expresión de enlace de datos `Item` está disponible y el control pasa a ser fuertemente tipado. Tal y como se ha mencionado en el tutorial anterior, puede seleccionar los detalles del objeto de elemento con IntelliSense, como especificar el `ProductName`:

![Mostrar datos de elementos y detalles - IntelliSense](display_data_items_and_details/_static/image1.png)

Además, se utiliza un enlace de modelo para especificar un `SelectMethod` valor. Este valor (`GetProducts`) se corresponderá con el método que va a agregar en el código subyacente para mostrar los productos en el paso siguiente.

### <a name="adding-code-to-display-products"></a>Agregar código para mostrar los productos

En este paso, agregará código para rellenar el **ListView** control con los datos de producto de la base de datos. El código será compatible con los productos que se muestra por categoría individual, así como que muestra todos los productos.

1. En **el Explorador de soluciones**, haga clic en *ProductList.aspx* y, a continuación, haga clic en **ver código**.
2. Reemplace el código existente en el *ProductList.aspx.cs* archivo con el código siguiente:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Este código muestra la `GetProducts` método que hace referencia el `ItemType` propiedad de la **ListView** controlar en el *ProductList.aspx* página. Para limitar los resultados a una categoría específica en la base de datos, el código establece la `categoryId` valor desde el valor de cadena de consulta pasado a la *ProductList.aspx* cuando el *ProductList.aspx* página se abrirá. El `QueryStringAttribute` clase en el `System.Web.ModelBinding` espacio de nombres se utiliza para recuperar el valor del identificador de variable de cadena de consulta. Esto indica el enlace de modelos para intentar enlazar un valor de la cadena de consulta para el `categoryId` parámetro en tiempo de ejecución.

Cuando una categoría válida se pasa como una cadena de consulta a la página, los resultados de la consulta están limitados a aquellos productos que pertenecen a la base de datos que coinciden con la `categoryId` valor. Por ejemplo, si la dirección URL para el *ProductsList.aspx* página es el siguiente:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

La página muestra solo los productos donde la `category` es igual a `1`.

Si no se incluye ninguna cadena de consulta cuando se desplace a la *ProductList.aspx* página, se mostrarán todos los productos.

Los orígenes de valores para estos métodos se conocen como *valor proveedores* (como *QueryString*), y los atributos de parámetro que indican un proveedor de valor que desea usar se conocen como valor atributos de proveedor (como "`id`"). ASP.NET incluye proveedores de valores y atributos correspondientes para todos los orígenes habituales de proporcionados por el usuario en una aplicación de formularios Web Forms, como la cadena de consulta, cookies, valores de formulario, controles, estado de vista, estado de sesión y propiedades de perfil. También puede escribir proveedores de valores personalizados.

### <a name="running-the-application"></a>Ejecutar la aplicación

Ejecute la aplicación ahora para ver cómo puede ver todos los productos o simplemente un conjunto de productos limitado por categoría.

1. En el **el Explorador de soluciones**, haga clic en el *Default.aspx* página y seleccione **ver en el explorador**.  
 El explorador se abrirá y mostrar el *Default.aspx* página.
2. Seleccione **automóviles** desde el menú de navegación de la categoría de producto.  
 El *ProductList.aspx* página se mostrará solo los productos incluidos en la categoría "Automóvil". Más adelante en este tutorial, se muestran detalles del producto.  

    ![Mostrar datos de elementos y detalles - automóviles](display_data_items_and_details/_static/image2.png)
3. Seleccione **productos** en el menú de navegación en la parte superior.  
 Una vez más, la *ProductList.aspx* aparece la página, pero esta vez muestra toda la lista de productos.   

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image3.png)
4. Cierre el explorador y vuelva a Visual Studio.

### <a name="adding-a-data-control-to-display-product-details"></a>Agregar un Control de datos para ver los detalles de producto

A continuación, modificará el marcado en el *ProductDetails.aspx* página que agregó en el tutorial anterior para que la página puede mostrar información sobre un producto individual.

1. En **el Explorador de soluciones**, abra el *ProductDetails.aspx* página.
2. Reemplace el marcado existente con el marcado siguiente:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Este código usa un **FormView** control para mostrar detalles sobre un producto individual. Este marcado utiliza métodos como los que se usan para mostrar los datos en el *ProductList.aspx* página. El **FormView** control se utiliza para mostrar un único registro a la vez desde un origen de datos. Cuando se usa el **FormView** (control), crear plantillas para mostrar y editar valores enlazados a datos. Las plantillas contienen controles, expresiones de enlace y formato que definen la apariencia y funcionalidad del formulario.

Para conectar el marcado anterior a la base de datos, debe agregar código adicional para la *ProductDetails.aspx* código.

1. En **el Explorador de soluciones**, haga clic en *ProductDetails.aspx* y, a continuación, haga clic en **ver código**.  
   El *ProductDetails.aspx.cs* se mostrará el archivo.
2. Reemplace el código existente con el siguiente código:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Este código comprueba si hay un "`productID`" valor de cadena de consulta. Si se encuentra un valor de cadena de consulta válida, se muestra el producto correspondiente. Si no se encuentra ninguna cadena de consulta o el valor de cadena de consulta no es válido, no se muestra ningún producto en el *ProductDetails.aspx* página.

### <a name="running-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para ver un producto individual que se muestra se basa en el identificador del producto.

1. Presione **F5** mientras se encuentre en Visual Studio para ejecutar la aplicación.  
 El explorador se abrirá y mostrar el *Default.aspx* página.
2. Seleccione "Barcos" en el menú de navegación de la categoría.  
 El *ProductList.aspx* se muestra la página.
3. Seleccione el producto "Papel barcos" de la lista de productos.  
 El *ProductDetails.aspx* se muestra la página.   

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image4.png)
4. Cierre el explorador.

## <a name="summary"></a>Resumen

En este tutorial de la serie se agregar marcado y código para mostrar una lista de productos y para mostrar los detalles del producto. Durante este proceso ha aprendido sobre los controles de datos fuertemente tipados, el enlace de modelos y proveedores de valores. En el siguiente tutorial, agregará un carro de la compra a la aplicación de ejemplo Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionales

[Recuperar y mostrar los datos con el enlace de modelos y formularios web forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Anterior](ui_and_navigation.md)
> [Siguiente](shopping-cart.md)
