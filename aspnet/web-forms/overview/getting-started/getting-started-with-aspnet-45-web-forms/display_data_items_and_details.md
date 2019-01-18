---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Datos para mostrar los elementos y detalles | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le mostrará los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.7 y Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396230"
---
<a name="display-data-items-and-details"></a>Mostrar los elementos de datos y detalles
====================
por [Erik Reitan](https://github.com/Erikre)

> Esta serie de tutoriales aprenderá los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.7 y Microsoft Visual Studio 2017.

En este tutorial, obtendrá información sobre cómo mostrar los elementos de datos y los detalles del elemento de datos con formularios Web Forms ASP.NET y Entity Framework Code First. Este tutorial se basa en el tutorial anterior de "Interfaz de usuario y navegación" como parte de la serie de tutoriales de Wingtip Toys Store. Después de completar este tutorial, podrá ver los productos en el *ProductsList.aspx* página y los detalles de un producto en el *ProductDetails.aspx* página.

## <a name="youll-learn-how-to"></a>Aprenderá a:

- Agregue un control de datos para mostrar los productos de la base de datos
- Conectar un control de datos a los datos seleccionados
- Agregue un control de datos para mostrar los detalles del producto de la base de datos
- Recuperar un valor de la cadena de consulta y use ese valor para limitar los datos que se recuperan de la base de datos

### <a name="features-introduced-in-this-tutorial"></a>Características presentadas en este tutorial:

- Enlace de modelos
- Proveedores de valor

## <a name="add-a-data-control"></a>Agregar un control de datos

Puede usar varias opciones diferentes para enlazar datos a un control de servidor. Entre los más comunes se incluyen:

 * Agregar un control de origen de datos
 * Agregar código a mano
 * Uso de enlace de modelos

### <a name="use-a-data-source-control-to-bind-data"></a>Utilizar un control de origen de datos para enlazar datos

Agrega un control de origen de datos, podrá vincular el control de origen de datos al control que muestra los datos. Con este enfoque, puede mediante declaración, en lugar de conectarse mediante programación, controles de servidor a los orígenes de datos.

### <a name="code-by-hand-to-bind-data"></a>Código a mano para enlazar datos

Codificar de manera manual implica:

1. Leer un valor
2. Comprobar si es null
3. Convierte en un tipo adecuado
4. Comprobación correcta de conversión
5. Mediante el valor de la consulta 

Este enfoque le permite tener control total sobre la lógica de acceso a datos.

### <a name="use-model-binding-to-bind-data"></a>Utilizar el enlace de modelos para enlazar datos

Enlace de modelos le permite enlazar los resultados con mucho menos código y le ofrece la capacidad de volver a usar la funcionalidad en toda la aplicación. Simplifica el trabajo con lógica de acceso a datos centrada en el código mientras sigue proporcionando un marco de enlace de datos enriquecido.

## <a name="display-products"></a>Mostrar los productos

En este tutorial, usará el enlace de modelos para enlazar datos. Para configurar un control de datos para utilizar el enlace de modelos para seleccionar datos, debe establecer el control `SelectMethod` propiedad a un nombre de método en el código de la página. El control de datos llama al método en el momento adecuado en el ciclo de vida de página y enlaza automáticamente los datos devueltos. No hace falta llamar explícitamente a la `DataBind` método.

1. En **el Explorador de soluciones**, abra *ProductList.aspx*.
2. Reemplace el marcado existente por este marcado:   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Este código usa un **ListView** control denominado `productList` para mostrar los productos.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Con los estilos y plantillas, puede definir el modo **ListView** control muestra los datos. Resulta útil para los datos en una estructura de repetición. Aunque esto **ListView** ejemplo simplemente muestra la base de datos, también puede, sin código, permiten a los usuarios editar, insertar y eliminar datos y para ordenar y paginar los datos.

Estableciendo el `ItemType` propiedad en el **ListView** controlar, la expresión de enlace de datos `Item` está disponible y el control pasa a ser inflexible. Como se mencionó en el tutorial anterior, puede seleccionar los detalles del objeto de elemento con IntelliSense, como especificar el `ProductName`:

![Mostrar datos de elementos y detalles - IntelliSense](display_data_items_and_details/_static/image1.png)

También usa el enlace de modelos para especificar un `SelectMethod` valor. Este valor (`GetProducts`) corresponde al método que se va a agregar al código de retraso para mostrar los productos en el paso siguiente.

### <a name="add-code-to-display-products"></a>Agregue código para mostrar los productos

En este paso, agregará código para rellenar el **ListView** control con datos de producto de la base de datos. El código es compatible con la que muestra todos los productos y productos de categorías individuales.

1. En **el Explorador de soluciones**, haga clic en *ProductList.aspx* y, a continuación, seleccione **ver código**.
2. Reemplace el código existente en el *ProductList.aspx.cs* archivo con esto:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Este código muestra la `GetProducts` método que el **ListView** del control `ItemType` referencias de propiedad en el *ProductList.aspx* página. Para limitar los resultados a una categoría específica de la base de datos, el código establece la `categoryId` valor desde el valor de cadena de consulta pasan a la *ProductList.aspx* página cuando la *ProductList.aspx* es la página navega. El `QueryStringAttribute` clase en el `System.Web.ModelBinding` espacio de nombres se usa para recuperar el valor de la variable de cadena de consulta `id`. Esto indica que el enlace de modelos para intentar enlazar un valor de cadena de consulta que el `categoryId` parámetro en tiempo de ejecución.

Cuando se pasa una categoría válida como una cadena de consulta a la página, los resultados de la consulta se limitan a esos productos en la base de datos que coinciden con la `categoryId` valor. Por ejemplo, si la *ProductsList.aspx* dirección URL de página es esto:


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

La página muestra solo los productos donde la `categoryId` es igual a `1`.

Todos los productos se muestran si no se incluye ninguna cadena de consulta cuando la *ProductList.aspx* se llama a la página.

Los orígenes de los valores de estos métodos se conocen como *proveedores de valor* (como *QueryString*), y los atributos de parámetro que indican qué proveedor de valor a utilizar se conocen como *atributos de proveedor de valor* (como `id`). ASP.NET incluye proveedores de valor y los atributos correspondientes para todos los orígenes de entrada de usuario habituales en una aplicación de formularios Web Forms, como la cadena de consulta, cookies, valores de formulario, controles, estado de vista, el estado de sesión y las propiedades de perfil. También puede escribir proveedores de valores personalizados.

### <a name="run-the-application"></a>Ejecutar la aplicación

Ejecute la aplicación ahora para ver todos los productos o productos de una categoría.

1. Presione **F5** mientras se encuentre en Visual Studio para ejecutar la aplicación.  
   El explorador se abre y muestra el *Default.aspx* página.

2. Seleccione **automóviles** desde el menú de navegación de la categoría de producto.  
   El *ProductList.aspx* página muestra solo muestra **automóviles** productos de la categoría. Más adelante en este tutorial, podrá mostrar detalles del producto.  

    ![Mostrar datos de elementos y detalles - automóviles](display_data_items_and_details/_static/image2.png)

3. Seleccione **productos** en el menú de navegación en la parte superior.  
   Nuevamente, el *ProductList.aspx* se muestra la página, pero esta vez muestra toda la lista de productos.   

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image3.png)

4. Cierre el explorador y vuelva a Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Agregue un control de datos para mostrar los detalles del producto

A continuación, modificará el marcado en el *ProductDetails.aspx* página que agregó en el tutorial anterior para mostrar información de producto específico.

1. En **el Explorador de soluciones**, abra *ProductDetails.aspx*.

2. Reemplace el marcado existente por este marcado:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Este código usa un **FormView** control para mostrar los detalles de producto específico. Este marcado usa métodos, como los métodos utilizados para mostrar datos en el *ProductList.aspx* página. El **FormView** control se usa para mostrar un único registro a la vez desde un origen de datos. Cuando se usa el **FormView** control, crea plantillas para mostrar y editar valores enlazados a datos. Estas plantillas contienen controles, enlace de expresiones, y el formato que definen la apariencia y la funcionalidad del formulario.

Conectar el marcado anterior a la base de datos requiere código adicional.

1. En **el Explorador de soluciones**, haga clic en *ProductDetails.aspx* y, a continuación, haga clic en **ver código**.  
   El *ProductDetails.aspx.cs* se muestra el archivo.

2. Reemplace el código existente con este código:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Este código comprueba si hay un "`productID`" valor de cadena de consulta. Si se encuentra un valor de cadena de consulta válida, se muestra el producto coincidente. Si no se encuentra la cadena de consulta o su valor no es válido, no se muestra ningún producto.

### <a name="run-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para ver un producto individual que se muestra según el identificador de producto.

1. Presione **F5** mientras se encuentre en Visual Studio para ejecutar la aplicación.  
   El explorador se abre y muestra el *Default.aspx* página.

2. Seleccione **barcos** en el menú de navegación de la categoría.  
   El *ProductList.aspx* se muestra la página.

3. Seleccione **papel barco** desde la lista de productos.
   El *ProductDetails.aspx* se muestra la página.

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image4.png)
    
4. Cierre el explorador.


## <a name="additional-resources"></a>Recursos adicionales

[Recuperar y mostrar datos con enlace de modelos y formularios web forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha agregado marcado y código para mostrar los productos y sus detalles. Ha aprendido acerca de los controles de datos fuertemente tipados, enlace de modelos y los proveedores de valor. En el siguiente tutorial, agregará un carro de la compra a la aplicación de ejemplo Wingtip Toys. 

> [!div class="step-by-step"]
> [Anterior](ui_and_navigation.md)
> [Siguiente](shopping-cart.md)
