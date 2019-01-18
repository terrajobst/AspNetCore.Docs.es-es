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
<a name="display-data-items-and-details"></a><span data-ttu-id="030ef-103">Mostrar los elementos de datos y detalles</span><span class="sxs-lookup"><span data-stu-id="030ef-103">Display data items and details</span></span>
====================
<span data-ttu-id="030ef-104">por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="030ef-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="030ef-105">Esta serie de tutoriales aprenderá los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.7 y Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="030ef-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="030ef-106">En este tutorial, obtendrá información sobre cómo mostrar los elementos de datos y los detalles del elemento de datos con formularios Web Forms ASP.NET y Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="030ef-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="030ef-107">Este tutorial se basa en el tutorial anterior de "Interfaz de usuario y navegación" como parte de la serie de tutoriales de Wingtip Toys Store.</span><span class="sxs-lookup"><span data-stu-id="030ef-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="030ef-108">Después de completar este tutorial, podrá ver los productos en el *ProductsList.aspx* página y los detalles de un producto en el *ProductDetails.aspx* página.</span><span class="sxs-lookup"><span data-stu-id="030ef-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="030ef-109">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="030ef-109">You'll learn how to:</span></span>

- <span data-ttu-id="030ef-110">Agregue un control de datos para mostrar los productos de la base de datos</span><span class="sxs-lookup"><span data-stu-id="030ef-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="030ef-111">Conectar un control de datos a los datos seleccionados</span><span class="sxs-lookup"><span data-stu-id="030ef-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="030ef-112">Agregue un control de datos para mostrar los detalles del producto de la base de datos</span><span class="sxs-lookup"><span data-stu-id="030ef-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="030ef-113">Recuperar un valor de la cadena de consulta y use ese valor para limitar los datos que se recuperan de la base de datos</span><span class="sxs-lookup"><span data-stu-id="030ef-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="030ef-114">Características presentadas en este tutorial:</span><span class="sxs-lookup"><span data-stu-id="030ef-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="030ef-115">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="030ef-115">Model binding</span></span>
- <span data-ttu-id="030ef-116">Proveedores de valor</span><span class="sxs-lookup"><span data-stu-id="030ef-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="030ef-117">Agregar un control de datos</span><span class="sxs-lookup"><span data-stu-id="030ef-117">Add a data control</span></span>

<span data-ttu-id="030ef-118">Puede usar varias opciones diferentes para enlazar datos a un control de servidor.</span><span class="sxs-lookup"><span data-stu-id="030ef-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="030ef-119">Entre los más comunes se incluyen:</span><span class="sxs-lookup"><span data-stu-id="030ef-119">The most common include:</span></span>

 * <span data-ttu-id="030ef-120">Agregar un control de origen de datos</span><span class="sxs-lookup"><span data-stu-id="030ef-120">Adding a data source control</span></span>
 * <span data-ttu-id="030ef-121">Agregar código a mano</span><span class="sxs-lookup"><span data-stu-id="030ef-121">Adding code by hand</span></span>
 * <span data-ttu-id="030ef-122">Uso de enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="030ef-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="030ef-123">Utilizar un control de origen de datos para enlazar datos</span><span class="sxs-lookup"><span data-stu-id="030ef-123">Use a data source control to bind data</span></span>

<span data-ttu-id="030ef-124">Agrega un control de origen de datos, podrá vincular el control de origen de datos al control que muestra los datos.</span><span class="sxs-lookup"><span data-stu-id="030ef-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="030ef-125">Con este enfoque, puede mediante declaración, en lugar de conectarse mediante programación, controles de servidor a los orígenes de datos.</span><span class="sxs-lookup"><span data-stu-id="030ef-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="030ef-126">Código a mano para enlazar datos</span><span class="sxs-lookup"><span data-stu-id="030ef-126">Code by hand to bind data</span></span>

<span data-ttu-id="030ef-127">Codificar de manera manual implica:</span><span class="sxs-lookup"><span data-stu-id="030ef-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="030ef-128">Leer un valor</span><span class="sxs-lookup"><span data-stu-id="030ef-128">Reading a value</span></span>
2. <span data-ttu-id="030ef-129">Comprobar si es null</span><span class="sxs-lookup"><span data-stu-id="030ef-129">Checking if it's null</span></span>
3. <span data-ttu-id="030ef-130">Convierte en un tipo adecuado</span><span class="sxs-lookup"><span data-stu-id="030ef-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="030ef-131">Comprobación correcta de conversión</span><span class="sxs-lookup"><span data-stu-id="030ef-131">Checking conversion success</span></span>
5. <span data-ttu-id="030ef-132">Mediante el valor de la consulta</span><span class="sxs-lookup"><span data-stu-id="030ef-132">Using the value in the query</span></span> 

<span data-ttu-id="030ef-133">Este enfoque le permite tener control total sobre la lógica de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="030ef-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="030ef-134">Utilizar el enlace de modelos para enlazar datos</span><span class="sxs-lookup"><span data-stu-id="030ef-134">Use model binding to bind data</span></span>

<span data-ttu-id="030ef-135">Enlace de modelos le permite enlazar los resultados con mucho menos código y le ofrece la capacidad de volver a usar la funcionalidad en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="030ef-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="030ef-136">Simplifica el trabajo con lógica de acceso a datos centrada en el código mientras sigue proporcionando un marco de enlace de datos enriquecido.</span><span class="sxs-lookup"><span data-stu-id="030ef-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="030ef-137">Mostrar los productos</span><span class="sxs-lookup"><span data-stu-id="030ef-137">Display products</span></span>

<span data-ttu-id="030ef-138">En este tutorial, usará el enlace de modelos para enlazar datos.</span><span class="sxs-lookup"><span data-stu-id="030ef-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="030ef-139">Para configurar un control de datos para utilizar el enlace de modelos para seleccionar datos, debe establecer el control `SelectMethod` propiedad a un nombre de método en el código de la página.</span><span class="sxs-lookup"><span data-stu-id="030ef-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="030ef-140">El control de datos llama al método en el momento adecuado en el ciclo de vida de página y enlaza automáticamente los datos devueltos.</span><span class="sxs-lookup"><span data-stu-id="030ef-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="030ef-141">No hace falta llamar explícitamente a la `DataBind` método.</span><span class="sxs-lookup"><span data-stu-id="030ef-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="030ef-142">En **el Explorador de soluciones**, abra *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="030ef-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="030ef-143">Reemplace el marcado existente por este marcado:</span><span class="sxs-lookup"><span data-stu-id="030ef-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="030ef-144">Este código usa un **ListView** control denominado `productList` para mostrar los productos.</span><span class="sxs-lookup"><span data-stu-id="030ef-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="030ef-145">Con los estilos y plantillas, puede definir el modo **ListView** control muestra los datos.</span><span class="sxs-lookup"><span data-stu-id="030ef-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="030ef-146">Resulta útil para los datos en una estructura de repetición.</span><span class="sxs-lookup"><span data-stu-id="030ef-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="030ef-147">Aunque esto **ListView** ejemplo simplemente muestra la base de datos, también puede, sin código, permiten a los usuarios editar, insertar y eliminar datos y para ordenar y paginar los datos.</span><span class="sxs-lookup"><span data-stu-id="030ef-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="030ef-148">Estableciendo el `ItemType` propiedad en el **ListView** controlar, la expresión de enlace de datos `Item` está disponible y el control pasa a ser inflexible.</span><span class="sxs-lookup"><span data-stu-id="030ef-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="030ef-149">Como se mencionó en el tutorial anterior, puede seleccionar los detalles del objeto de elemento con IntelliSense, como especificar el `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="030ef-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Mostrar datos de elementos y detalles - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="030ef-151">También usa el enlace de modelos para especificar un `SelectMethod` valor.</span><span class="sxs-lookup"><span data-stu-id="030ef-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="030ef-152">Este valor (`GetProducts`) corresponde al método que se va a agregar al código de retraso para mostrar los productos en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="030ef-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="030ef-153">Agregue código para mostrar los productos</span><span class="sxs-lookup"><span data-stu-id="030ef-153">Add code to display products</span></span>

<span data-ttu-id="030ef-154">En este paso, agregará código para rellenar el **ListView** control con datos de producto de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="030ef-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="030ef-155">El código es compatible con la que muestra todos los productos y productos de categorías individuales.</span><span class="sxs-lookup"><span data-stu-id="030ef-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="030ef-156">En **el Explorador de soluciones**, haga clic en *ProductList.aspx* y, a continuación, seleccione **ver código**.</span><span class="sxs-lookup"><span data-stu-id="030ef-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="030ef-157">Reemplace el código existente en el *ProductList.aspx.cs* archivo con esto:</span><span class="sxs-lookup"><span data-stu-id="030ef-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="030ef-158">Este código muestra la `GetProducts` método que el **ListView** del control `ItemType` referencias de propiedad en el *ProductList.aspx* página.</span><span class="sxs-lookup"><span data-stu-id="030ef-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="030ef-159">Para limitar los resultados a una categoría específica de la base de datos, el código establece la `categoryId` valor desde el valor de cadena de consulta pasan a la *ProductList.aspx* página cuando la *ProductList.aspx* es la página navega.</span><span class="sxs-lookup"><span data-stu-id="030ef-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="030ef-160">El `QueryStringAttribute` clase en el `System.Web.ModelBinding` espacio de nombres se usa para recuperar el valor de la variable de cadena de consulta `id`.</span><span class="sxs-lookup"><span data-stu-id="030ef-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="030ef-161">Esto indica que el enlace de modelos para intentar enlazar un valor de cadena de consulta que el `categoryId` parámetro en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="030ef-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="030ef-162">Cuando se pasa una categoría válida como una cadena de consulta a la página, los resultados de la consulta se limitan a esos productos en la base de datos que coinciden con la `categoryId` valor.</span><span class="sxs-lookup"><span data-stu-id="030ef-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="030ef-163">Por ejemplo, si la *ProductsList.aspx* dirección URL de página es esto:</span><span class="sxs-lookup"><span data-stu-id="030ef-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="030ef-164">La página muestra solo los productos donde la `categoryId` es igual a `1`.</span><span class="sxs-lookup"><span data-stu-id="030ef-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="030ef-165">Todos los productos se muestran si no se incluye ninguna cadena de consulta cuando la *ProductList.aspx* se llama a la página.</span><span class="sxs-lookup"><span data-stu-id="030ef-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="030ef-166">Los orígenes de los valores de estos métodos se conocen como *proveedores de valor* (como *QueryString*), y los atributos de parámetro que indican qué proveedor de valor a utilizar se conocen como *atributos de proveedor de valor* (como `id`).</span><span class="sxs-lookup"><span data-stu-id="030ef-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="030ef-167">ASP.NET incluye proveedores de valor y los atributos correspondientes para todos los orígenes de entrada de usuario habituales en una aplicación de formularios Web Forms, como la cadena de consulta, cookies, valores de formulario, controles, estado de vista, el estado de sesión y las propiedades de perfil.</span><span class="sxs-lookup"><span data-stu-id="030ef-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="030ef-168">También puede escribir proveedores de valores personalizados.</span><span class="sxs-lookup"><span data-stu-id="030ef-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="030ef-169">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="030ef-169">Run the application</span></span>

<span data-ttu-id="030ef-170">Ejecute la aplicación ahora para ver todos los productos o productos de una categoría.</span><span class="sxs-lookup"><span data-stu-id="030ef-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="030ef-171">Presione **F5** mientras se encuentre en Visual Studio para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="030ef-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="030ef-172">El explorador se abre y muestra el *Default.aspx* página.</span><span class="sxs-lookup"><span data-stu-id="030ef-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="030ef-173">Seleccione **automóviles** desde el menú de navegación de la categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="030ef-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="030ef-174">El *ProductList.aspx* página muestra solo muestra **automóviles** productos de la categoría.</span><span class="sxs-lookup"><span data-stu-id="030ef-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="030ef-175">Más adelante en este tutorial, podrá mostrar detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="030ef-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Mostrar datos de elementos y detalles - automóviles](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="030ef-177">Seleccione **productos** en el menú de navegación en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="030ef-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="030ef-178">Nuevamente, el *ProductList.aspx* se muestra la página, pero esta vez muestra toda la lista de productos.</span><span class="sxs-lookup"><span data-stu-id="030ef-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="030ef-180">Cierre el explorador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="030ef-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="030ef-181">Agregue un control de datos para mostrar los detalles del producto</span><span class="sxs-lookup"><span data-stu-id="030ef-181">Add a data control to display product details</span></span>

<span data-ttu-id="030ef-182">A continuación, modificará el marcado en el *ProductDetails.aspx* página que agregó en el tutorial anterior para mostrar información de producto específico.</span><span class="sxs-lookup"><span data-stu-id="030ef-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="030ef-183">En **el Explorador de soluciones**, abra *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="030ef-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="030ef-184">Reemplace el marcado existente por este marcado:</span><span class="sxs-lookup"><span data-stu-id="030ef-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="030ef-185">Este código usa un **FormView** control para mostrar los detalles de producto específico.</span><span class="sxs-lookup"><span data-stu-id="030ef-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="030ef-186">Este marcado usa métodos, como los métodos utilizados para mostrar datos en el *ProductList.aspx* página.</span><span class="sxs-lookup"><span data-stu-id="030ef-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="030ef-187">El **FormView** control se usa para mostrar un único registro a la vez desde un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="030ef-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="030ef-188">Cuando se usa el **FormView** control, crea plantillas para mostrar y editar valores enlazados a datos.</span><span class="sxs-lookup"><span data-stu-id="030ef-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="030ef-189">Estas plantillas contienen controles, enlace de expresiones, y el formato que definen la apariencia y la funcionalidad del formulario.</span><span class="sxs-lookup"><span data-stu-id="030ef-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="030ef-190">Conectar el marcado anterior a la base de datos requiere código adicional.</span><span class="sxs-lookup"><span data-stu-id="030ef-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="030ef-191">En **el Explorador de soluciones**, haga clic en *ProductDetails.aspx* y, a continuación, haga clic en **ver código**.</span><span class="sxs-lookup"><span data-stu-id="030ef-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="030ef-192">El *ProductDetails.aspx.cs* se muestra el archivo.</span><span class="sxs-lookup"><span data-stu-id="030ef-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="030ef-193">Reemplace el código existente con este código:</span><span class="sxs-lookup"><span data-stu-id="030ef-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="030ef-194">Este código comprueba si hay un "`productID`" valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="030ef-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="030ef-195">Si se encuentra un valor de cadena de consulta válida, se muestra el producto coincidente.</span><span class="sxs-lookup"><span data-stu-id="030ef-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="030ef-196">Si no se encuentra la cadena de consulta o su valor no es válido, no se muestra ningún producto.</span><span class="sxs-lookup"><span data-stu-id="030ef-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="030ef-197">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="030ef-197">Run the application</span></span>

<span data-ttu-id="030ef-198">Ahora puede ejecutar la aplicación para ver un producto individual que se muestra según el identificador de producto.</span><span class="sxs-lookup"><span data-stu-id="030ef-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="030ef-199">Presione **F5** mientras se encuentre en Visual Studio para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="030ef-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="030ef-200">El explorador se abre y muestra el *Default.aspx* página.</span><span class="sxs-lookup"><span data-stu-id="030ef-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="030ef-201">Seleccione **barcos** en el menú de navegación de la categoría.</span><span class="sxs-lookup"><span data-stu-id="030ef-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="030ef-202">El *ProductList.aspx* se muestra la página.</span><span class="sxs-lookup"><span data-stu-id="030ef-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="030ef-203">Seleccione **papel barco** desde la lista de productos.</span><span class="sxs-lookup"><span data-stu-id="030ef-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="030ef-204">El *ProductDetails.aspx* se muestra la página.</span><span class="sxs-lookup"><span data-stu-id="030ef-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="030ef-206">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="030ef-206">Close the browser.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="030ef-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="030ef-207">Additional resources</span></span>

[<span data-ttu-id="030ef-208">Recuperar y mostrar datos con enlace de modelos y formularios web forms</span><span class="sxs-lookup"><span data-stu-id="030ef-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="030ef-209">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="030ef-209">Next steps</span></span>

<span data-ttu-id="030ef-210">En este tutorial, ha agregado marcado y código para mostrar los productos y sus detalles.</span><span class="sxs-lookup"><span data-stu-id="030ef-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="030ef-211">Ha aprendido acerca de los controles de datos fuertemente tipados, enlace de modelos y los proveedores de valor.</span><span class="sxs-lookup"><span data-stu-id="030ef-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="030ef-212">En el siguiente tutorial, agregará un carro de la compra a la aplicación de ejemplo Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="030ef-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="030ef-213">[Anterior](ui_and_navigation.md)
> [Siguiente](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="030ef-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
