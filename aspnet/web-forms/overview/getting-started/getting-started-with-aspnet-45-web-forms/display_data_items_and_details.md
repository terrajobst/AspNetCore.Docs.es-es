---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Datos para mostrar los elementos y detalles | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.7 y Microsoft Visual Studio Community 2017 para Web
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207439"
---
<a name="display-data-items-and-details"></a>Mostrar los elementos de datos y detalles
====================
por [Erik Reitan](https://github.com/Erikre)

> Esta serie de tutoriales aprenderá los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.7 y Microsoft Visual Studio Community 2017 para Web.

En este tutorial, obtendrá información sobre cómo mostrar los elementos de datos y los detalles del elemento de datos con formularios Web Forms ASP.NET y Entity Framework Code First. Este tutorial se basa en el tutorial anterior de "Interfaz de usuario y navegación" como parte de la serie de tutoriales de Wingtip Toys Store. En el tutorial completo, los productos en el *ProductsList.aspx* página y los detalles de un producto en el *ProductDetails.aspx* página se muestran.

## <a name="what-you-learn"></a>¿Qué aprenderá

- Agregue un control de datos para mostrar los productos de base de datos.
- Conectar un control de datos a los datos seleccionados.
- Agregue un control de datos para mostrar los detalles del producto.
- Analizar un valor de cadena de consulta y usarla para filtrar los datos recuperados de la base de datos.

Características presentadas en este tutorial incluyen el enlace de modelos y los proveedores de valor.

## <a name="add-a-data-control-to-display-products"></a>Agregue un control de datos para mostrar los productos
 
Tiene unas cuantas opciones para enlazar datos a un control de servidor. Entre los más comunes se incluyen:

 * Agregar un control de origen de datos
 * Agregar código a mano
 * Enlace de modelos de implementación

### <a name="use-a-data-source-control-to-bind-data"></a>Utilizar un control de origen de datos para enlazar datos

Al agregar un control de origen de datos, vincula el control de origen de datos al control que muestra los datos. Con este enfoque, puede mediante declaración, en lugar de conectarse mediante programación, controles de servidor a los orígenes de datos.

### <a name="code-by-hand-to-bind-data"></a>Código a mano para enlazar datos

Codificar de manera manual implica:

1. Leer un valor
2. Comprobar si es null
3. Convierte en un tipo adecuado
4. Comprobación correcta de conversión
5. Realizar una consulta con el valor convertido 

Con este enfoque, tiene control total sobre la lógica de acceso a datos.

### <a name="use-model-binding-to-bind-data"></a>Utilizar el enlace de modelos para enlazar datos

Con el enlace de modelos, enlazar los resultados con mucho menos código y ofrece la capacidad de volver a usar la funcionalidad en toda la aplicación. Simplifica el trabajo con lógica de acceso a datos centrada en el código mientras sigue proporcionando un marco de enlace de datos enriquecido.

## <a name="display-products"></a>Mostrar los productos

En este tutorial, utiliza el enlace de modelos para enlazar datos. Para configurar un control de datos para utilizar el enlace de modelos para seleccionar datos, debe establecer el control `SelectMethod` propiedad a un método en el código de la página. El control de datos llama al método en el momento adecuado en el ciclo de vida de página y enlaza automáticamente los datos devueltos. No hace falta llamar explícitamente a la `DataBind` método.

Trabajar con los pasos siguientes, modificar *ProductList.aspx* marcado para mostrar productos.

1. En **el Explorador de soluciones**, abra *ProductList.aspx*.

2. Reemplace el marcado existente por el siguiente marcado: 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

El marcado anterior usa un **ListView** control denominado `productList` para mostrar los productos.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Con los estilos y plantillas, puede definir el modo **ListView** control muestra los datos. Resulta útil para los datos en una estructura de repetición. Aunque esto **ListView** ejemplo simplemente muestra la base de datos, también puede, sin código, permiten a los usuarios editar, insertar y eliminar datos y para ordenar y paginar los datos.

Al establecer el `ItemType` propiedad en el **ListView** controlar, la expresión de enlace de datos `Item` está disponible y el control pasa a ser inflexible. Como se mencionó en el tutorial anterior, puede seleccionar los detalles del objeto de elemento con IntelliSense, como especificar el `ProductName`:

![Mostrar datos de elementos y detalles - IntelliSense](display_data_items_and_details/_static/image1.png)

Con el enlace de modelos, debe especificar un `SelectMethod` valor (`GetProducts`). Este es el método que agregue al código de retraso para mostrar los productos en el paso siguiente.

### <a name="add-code-to-display-products"></a>Agregue código para mostrar los productos

En este paso, agregará código para rellenar el **ListView** control con datos de productos de base de datos. El código es compatible con la que muestra todos los productos y productos de categorías individuales.

1. En **el Explorador de soluciones**, haga clic en *ProductList.aspx* y, a continuación, seleccione **ver código**.
2. Reemplace el código existente en el *ProductList.aspx.cs* archivo con esto:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Este código muestra la `GetProducts` método que el **ListView** del control `ItemType` referencias de propiedad en *ProductList.aspx*. Para limitar los resultados a una categoría específica de la base de datos, el código establece la `categoryId` valor de la cadena de consulta pasada a *ProductList.aspx*. El `QueryStringAttribute` clase en el `System.Web.ModelBinding` espacio de nombres se usa para recuperar la variable de cadena de consulta `id`del valor. Esto indica que el enlace de modelos para, en tiempo de ejecución, enlazar un valor de cadena de consulta para el `categoryId` parámetro.

Cuando una categoría válida (`categoryId`) es pasado, los resultados se limitan a los productos de base de datos de esa categoría. Por ejemplo, si la *ProductsList.aspx* dirección URL de página es esto:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

La página muestra solo los productos donde la `categoryId` es igual a `1`.

Si no se pasa ninguna cadena de consulta, se muestran todos los productos.

Los orígenes de los valores para estos métodos se denominan *proveedores de valor* (como `QueryString`), y se denominan los atributos de parámetro que indican qué proveedor de valor a utilizar *atributos de proveedor de valor* () como `id`). ASP.NET incluye proveedores de valor y los atributos de todos los formularios Web Forms aplicación usuario entrados orígenes habituales. Estos incluyen la cadena de consulta, cookies, valores de formulario, controles, estado de vista, el estado de sesión y las propiedades de perfil. También puede escribir proveedores de valores personalizados.

### <a name="run-the-application"></a>Ejecutar la aplicación

Ejecute la aplicación ahora para ver todos los productos o productos de una categoría.

1. En Visual Studio, presione **F5** para ejecutar la aplicación.
 El explorador se abre y muestra el *Default.aspx* página.

2. En el menú de categoría de producto, seleccione **automóviles**.

   El *ProductList.aspx* aparece la página, que muestra solo productos de la **automóviles** categoría. Más adelante en este tutorial, mostrar detalles del producto.

    ![Mostrar datos de elementos y detalles - automóviles](display_data_items_and_details/_static/image2.png)

3. Seleccione **productos** en el menú superior.
 El *ProductList.aspx* página ahora muestra todos los productos. 

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image3.png)

4. Cierre el explorador y vuelva a Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Agregue un Control de datos para mostrar los detalles del producto

Modificar el *ProductDetails.aspx* marcado que agregó en el tutorial anterior para mostrar información de producto específico:

1. En **el Explorador de soluciones**, abra *ProductDetails.aspx*.

2. Reemplace el marcado existente por este marcado:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

Este marcado usa un **FormView** control para mostrar los detalles de producto específico. Utiliza métodos como los que se usan para mostrar datos en *ProductList.aspx*. El **FormView** control se usa para mostrar un único registro a la vez desde un origen de datos. Cuando se usa el **FormView** control, crea plantillas para mostrar y editar valores enlazados a datos. Estas plantillas contienen controles, enlace de expresiones, y el formato que definen la apariencia y la funcionalidad del formulario.

Conectar el marcado anterior a la base de datos requiere código adicional.

1. En **el Explorador de soluciones**, haga clic en *ProductDetails.aspx* y, a continuación, seleccione **ver código**.  
   El *ProductDetails.aspx.cs* se muestra el archivo.

2. Reemplace el código existente con este:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Este código comprueba si hay un "`productID`" valor de cadena de consulta. Si se encuentra un valor válido, se muestra el producto coincidente. Si no se encuentra la cadena de consulta o su valor no es válido, no se muestra ningún producto.

### <a name="run-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para ver los detalles de producto específico según el identificador de producto.

1. En Visual Studio, presione **F5** para ejecutar la aplicación.  
 El explorador se abre en *Default.aspx*.

2. En el menú de la categoría, seleccione **barcos**.  
 El *ProductList.aspx* se muestra la página.

3. Seleccione **papel barco**.  
 El *ProductDetails.aspx* se muestra la página.   

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image4.png)

En el siguiente tutorial, agregará un carro de la compra a la aplicación Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionales

[Recuperar y mostrar datos con enlace de modelos y formularios web forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Anterior](ui_and_navigation.md)
> [Siguiente](shopping-cart.md)
