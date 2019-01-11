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
<a name="display-data-items-and-details"></a><span data-ttu-id="78f53-103">Mostrar los elementos de datos y detalles</span><span class="sxs-lookup"><span data-stu-id="78f53-103">Display data items and details</span></span>
====================
<span data-ttu-id="78f53-104">por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="78f53-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="78f53-105">Esta serie de tutoriales aprenderá los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.7 y Microsoft Visual Studio Community 2017 para Web.</span><span class="sxs-lookup"><span data-stu-id="78f53-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio Community 2017 for the Web.</span></span>

<span data-ttu-id="78f53-106">En este tutorial, obtendrá información sobre cómo mostrar los elementos de datos y los detalles del elemento de datos con formularios Web Forms ASP.NET y Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="78f53-106">In this tutorial, you learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="78f53-107">Este tutorial se basa en el tutorial anterior de "Interfaz de usuario y navegación" como parte de la serie de tutoriales de Wingtip Toys Store.</span><span class="sxs-lookup"><span data-stu-id="78f53-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="78f53-108">En el tutorial completo, los productos en el *ProductsList.aspx* página y los detalles de un producto en el *ProductDetails.aspx* página se muestran.</span><span class="sxs-lookup"><span data-stu-id="78f53-108">In the completed tutorial, products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page are displayed.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="78f53-109">¿Qué aprenderá</span><span class="sxs-lookup"><span data-stu-id="78f53-109">What you learn</span></span>

- <span data-ttu-id="78f53-110">Agregue un control de datos para mostrar los productos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-110">Add a data control to display database products.</span></span>
- <span data-ttu-id="78f53-111">Conectar un control de datos a los datos seleccionados.</span><span class="sxs-lookup"><span data-stu-id="78f53-111">Connect a data control to selected data.</span></span>
- <span data-ttu-id="78f53-112">Agregue un control de datos para mostrar los detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="78f53-112">Add a data control to display product details.</span></span>
- <span data-ttu-id="78f53-113">Analizar un valor de cadena de consulta y usarla para filtrar los datos recuperados de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-113">Parse a query string value and use it to filter retrieved database data.</span></span>

<span data-ttu-id="78f53-114">Características presentadas en este tutorial incluyen el enlace de modelos y los proveedores de valor.</span><span class="sxs-lookup"><span data-stu-id="78f53-114">Features introduced in this tutorial include model binding and value providers.</span></span>

## <a name="add-a-data-control-to-display-products"></a><span data-ttu-id="78f53-115">Agregue un control de datos para mostrar los productos</span><span class="sxs-lookup"><span data-stu-id="78f53-115">Add a data control to display products</span></span>
 
<span data-ttu-id="78f53-116">Tiene unas cuantas opciones para enlazar datos a un control de servidor.</span><span class="sxs-lookup"><span data-stu-id="78f53-116">You have a few options to bind data to a server control.</span></span> <span data-ttu-id="78f53-117">Entre los más comunes se incluyen:</span><span class="sxs-lookup"><span data-stu-id="78f53-117">The most common include:</span></span>

 * <span data-ttu-id="78f53-118">Agregar un control de origen de datos</span><span class="sxs-lookup"><span data-stu-id="78f53-118">Adding a data source control</span></span>
 * <span data-ttu-id="78f53-119">Agregar código a mano</span><span class="sxs-lookup"><span data-stu-id="78f53-119">Adding code by hand</span></span>
 * <span data-ttu-id="78f53-120">Enlace de modelos de implementación</span><span class="sxs-lookup"><span data-stu-id="78f53-120">Implementing model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="78f53-121">Utilizar un control de origen de datos para enlazar datos</span><span class="sxs-lookup"><span data-stu-id="78f53-121">Use a data source control to bind data</span></span>

<span data-ttu-id="78f53-122">Al agregar un control de origen de datos, vincula el control de origen de datos al control que muestra los datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-122">Adding a data source control links the data source control to the control that displays the data.</span></span> <span data-ttu-id="78f53-123">Con este enfoque, puede mediante declaración, en lugar de conectarse mediante programación, controles de servidor a los orígenes de datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-123">With this approach, you can declaratively, rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="78f53-124">Código a mano para enlazar datos</span><span class="sxs-lookup"><span data-stu-id="78f53-124">Code by hand to bind data</span></span>

<span data-ttu-id="78f53-125">Codificar de manera manual implica:</span><span class="sxs-lookup"><span data-stu-id="78f53-125">Coding by hand involves:</span></span>

1. <span data-ttu-id="78f53-126">Leer un valor</span><span class="sxs-lookup"><span data-stu-id="78f53-126">Reading a value</span></span>
2. <span data-ttu-id="78f53-127">Comprobar si es null</span><span class="sxs-lookup"><span data-stu-id="78f53-127">Checking if it's null</span></span>
3. <span data-ttu-id="78f53-128">Convierte en un tipo adecuado</span><span class="sxs-lookup"><span data-stu-id="78f53-128">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="78f53-129">Comprobación correcta de conversión</span><span class="sxs-lookup"><span data-stu-id="78f53-129">Checking conversion success</span></span>
5. <span data-ttu-id="78f53-130">Realizar una consulta con el valor convertido</span><span class="sxs-lookup"><span data-stu-id="78f53-130">Making a query with the converted value</span></span> 

<span data-ttu-id="78f53-131">Con este enfoque, tiene control total sobre la lógica de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-131">With this approach, you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="78f53-132">Utilizar el enlace de modelos para enlazar datos</span><span class="sxs-lookup"><span data-stu-id="78f53-132">Use model binding to bind data</span></span>

<span data-ttu-id="78f53-133">Con el enlace de modelos, enlazar los resultados con mucho menos código y ofrece la capacidad de volver a usar la funcionalidad en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="78f53-133">With model binding, you bind results with far less code and it gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="78f53-134">Simplifica el trabajo con lógica de acceso a datos centrada en el código mientras sigue proporcionando un marco de enlace de datos enriquecido.</span><span class="sxs-lookup"><span data-stu-id="78f53-134">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="78f53-135">Mostrar los productos</span><span class="sxs-lookup"><span data-stu-id="78f53-135">Display products</span></span>

<span data-ttu-id="78f53-136">En este tutorial, utiliza el enlace de modelos para enlazar datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-136">In this tutorial, you use model binding to bind data.</span></span> <span data-ttu-id="78f53-137">Para configurar un control de datos para utilizar el enlace de modelos para seleccionar datos, debe establecer el control `SelectMethod` propiedad a un método en el código de la página.</span><span class="sxs-lookup"><span data-stu-id="78f53-137">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method in the page's code.</span></span> <span data-ttu-id="78f53-138">El control de datos llama al método en el momento adecuado en el ciclo de vida de página y enlaza automáticamente los datos devueltos.</span><span class="sxs-lookup"><span data-stu-id="78f53-138">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="78f53-139">No hace falta llamar explícitamente a la `DataBind` método.</span><span class="sxs-lookup"><span data-stu-id="78f53-139">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="78f53-140">Trabajar con los pasos siguientes, modificar *ProductList.aspx* marcado para mostrar productos.</span><span class="sxs-lookup"><span data-stu-id="78f53-140">Working through the following steps, you modify *ProductList.aspx* markup to display products.</span></span>

1. <span data-ttu-id="78f53-141">En **el Explorador de soluciones**, abra *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="78f53-141">In **Solution Explorer**, open *ProductList.aspx*.</span></span>

2. <span data-ttu-id="78f53-142">Reemplace el marcado existente por el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="78f53-142">Replace the existing markup with the following markup:</span></span> 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="78f53-143">El marcado anterior usa un **ListView** control denominado `productList` para mostrar los productos.</span><span class="sxs-lookup"><span data-stu-id="78f53-143">The preceding markup uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="78f53-144">Con los estilos y plantillas, puede definir el modo **ListView** control muestra los datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-144">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="78f53-145">Resulta útil para los datos en una estructura de repetición.</span><span class="sxs-lookup"><span data-stu-id="78f53-145">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="78f53-146">Aunque esto **ListView** ejemplo simplemente muestra la base de datos, también puede, sin código, permiten a los usuarios editar, insertar y eliminar datos y para ordenar y paginar los datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-146">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="78f53-147">Al establecer el `ItemType` propiedad en el **ListView** controlar, la expresión de enlace de datos `Item` está disponible y el control pasa a ser inflexible.</span><span class="sxs-lookup"><span data-stu-id="78f53-147">When you set the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="78f53-148">Como se mencionó en el tutorial anterior, puede seleccionar los detalles del objeto de elemento con IntelliSense, como especificar el `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="78f53-148">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Mostrar datos de elementos y detalles - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="78f53-150">Con el enlace de modelos, debe especificar un `SelectMethod` valor (`GetProducts`).</span><span class="sxs-lookup"><span data-stu-id="78f53-150">With model binding, you're specifying a `SelectMethod` value (`GetProducts`).</span></span> <span data-ttu-id="78f53-151">Este es el método que agregue al código de retraso para mostrar los productos en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="78f53-151">This is the method you add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="78f53-152">Agregue código para mostrar los productos</span><span class="sxs-lookup"><span data-stu-id="78f53-152">Add code to display products</span></span>

<span data-ttu-id="78f53-153">En este paso, agregará código para rellenar el **ListView** control con datos de productos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-153">In this step, you add code to populate the **ListView** control with database product data.</span></span> <span data-ttu-id="78f53-154">El código es compatible con la que muestra todos los productos y productos de categorías individuales.</span><span class="sxs-lookup"><span data-stu-id="78f53-154">The code supports showing all products and individual category products.</span></span>

1. <span data-ttu-id="78f53-155">En **el Explorador de soluciones**, haga clic en *ProductList.aspx* y, a continuación, seleccione **ver código**.</span><span class="sxs-lookup"><span data-stu-id="78f53-155">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="78f53-156">Reemplace el código existente en el *ProductList.aspx.cs* archivo con esto:</span><span class="sxs-lookup"><span data-stu-id="78f53-156">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="78f53-157">Este código muestra la `GetProducts` método que el **ListView** del control `ItemType` referencias de propiedad en *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="78f53-157">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in *ProductList.aspx*.</span></span> <span data-ttu-id="78f53-158">Para limitar los resultados a una categoría específica de la base de datos, el código establece la `categoryId` valor de la cadena de consulta pasada a *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="78f53-158">To limit the results to a specific database category, the code sets the `categoryId` value from the query string passed to *ProductList.aspx*.</span></span> <span data-ttu-id="78f53-159">El `QueryStringAttribute` clase en el `System.Web.ModelBinding` espacio de nombres se usa para recuperar la variable de cadena de consulta `id`del valor.</span><span class="sxs-lookup"><span data-stu-id="78f53-159">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the query string variable `id`'s value.</span></span> <span data-ttu-id="78f53-160">Esto indica que el enlace de modelos para, en tiempo de ejecución, enlazar un valor de cadena de consulta para el `categoryId` parámetro.</span><span class="sxs-lookup"><span data-stu-id="78f53-160">This instructs model binding to, at run time, bind a query string value to the `categoryId` parameter.</span></span>

<span data-ttu-id="78f53-161">Cuando una categoría válida (`categoryId`) es pasado, los resultados se limitan a los productos de base de datos de esa categoría.</span><span class="sxs-lookup"><span data-stu-id="78f53-161">When a valid category (`categoryId`) is passed, the results are limited to that category's database products.</span></span> <span data-ttu-id="78f53-162">Por ejemplo, si la *ProductsList.aspx* dirección URL de página es esto:</span><span class="sxs-lookup"><span data-stu-id="78f53-162">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="78f53-163">La página muestra solo los productos donde la `categoryId` es igual a `1`.</span><span class="sxs-lookup"><span data-stu-id="78f53-163">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="78f53-164">Si no se pasa ninguna cadena de consulta, se muestran todos los productos.</span><span class="sxs-lookup"><span data-stu-id="78f53-164">All products are displayed if no query string is passed.</span></span>

<span data-ttu-id="78f53-165">Los orígenes de los valores para estos métodos se denominan *proveedores de valor* (como `QueryString`), y se denominan los atributos de parámetro que indican qué proveedor de valor a utilizar *atributos de proveedor de valor* () como `id`).</span><span class="sxs-lookup"><span data-stu-id="78f53-165">The value sources for these methods are called *value providers* (such as `QueryString`), and the parameter attributes that indicate which value provider to use are called *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="78f53-166">ASP.NET incluye proveedores de valor y los atributos de todos los formularios Web Forms aplicación usuario entrados orígenes habituales.</span><span class="sxs-lookup"><span data-stu-id="78f53-166">ASP.NET includes value providers and attributes for all typical Web Forms application user input sources.</span></span> <span data-ttu-id="78f53-167">Estos incluyen la cadena de consulta, cookies, valores de formulario, controles, estado de vista, el estado de sesión y las propiedades de perfil.</span><span class="sxs-lookup"><span data-stu-id="78f53-167">These include the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="78f53-168">También puede escribir proveedores de valores personalizados.</span><span class="sxs-lookup"><span data-stu-id="78f53-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="78f53-169">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="78f53-169">Run the application</span></span>

<span data-ttu-id="78f53-170">Ejecute la aplicación ahora para ver todos los productos o productos de una categoría.</span><span class="sxs-lookup"><span data-stu-id="78f53-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="78f53-171">En Visual Studio, presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="78f53-171">In Visual Studio, press **F5** to run the application.</span></span>
 <span data-ttu-id="78f53-172">El explorador se abre y muestra el *Default.aspx* página.</span><span class="sxs-lookup"><span data-stu-id="78f53-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="78f53-173">En el menú de categoría de producto, seleccione **automóviles**.</span><span class="sxs-lookup"><span data-stu-id="78f53-173">From the product category menu, select **Cars**.</span></span>

   <span data-ttu-id="78f53-174">El *ProductList.aspx* aparece la página, que muestra solo productos de la **automóviles** categoría.</span><span class="sxs-lookup"><span data-stu-id="78f53-174">The *ProductList.aspx* page appears, showing only products from the **Cars** category.</span></span> <span data-ttu-id="78f53-175">Más adelante en este tutorial, mostrar detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="78f53-175">Later in this tutorial, you display product details.</span></span>

    ![Mostrar datos de elementos y detalles - automóviles](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="78f53-177">Seleccione **productos** en el menú superior.</span><span class="sxs-lookup"><span data-stu-id="78f53-177">Select **Products** from the top menu.</span></span>
 <span data-ttu-id="78f53-178">El *ProductList.aspx* página ahora muestra todos los productos.</span><span class="sxs-lookup"><span data-stu-id="78f53-178">The *ProductList.aspx* page now displays all products.</span></span> 

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="78f53-180">Cierre el explorador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78f53-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="78f53-181">Agregue un Control de datos para mostrar los detalles del producto</span><span class="sxs-lookup"><span data-stu-id="78f53-181">Add a Data Control to display product details</span></span>

<span data-ttu-id="78f53-182">Modificar el *ProductDetails.aspx* marcado que agregó en el tutorial anterior para mostrar información de producto específico:</span><span class="sxs-lookup"><span data-stu-id="78f53-182">Modify the *ProductDetails.aspx* markup that you added in the previous tutorial to display specific product information:</span></span>

1. <span data-ttu-id="78f53-183">En **el Explorador de soluciones**, abra *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="78f53-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="78f53-184">Reemplace el marcado existente por este marcado:</span><span class="sxs-lookup"><span data-stu-id="78f53-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

<span data-ttu-id="78f53-185">Este marcado usa un **FormView** control para mostrar los detalles de producto específico.</span><span class="sxs-lookup"><span data-stu-id="78f53-185">This markup uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="78f53-186">Utiliza métodos como los que se usan para mostrar datos en *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="78f53-186">It uses methods like those used to display data in *ProductList.aspx*.</span></span> <span data-ttu-id="78f53-187">El **FormView** control se usa para mostrar un único registro a la vez desde un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="78f53-188">Cuando se usa el **FormView** control, crea plantillas para mostrar y editar valores enlazados a datos.</span><span class="sxs-lookup"><span data-stu-id="78f53-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="78f53-189">Estas plantillas contienen controles, enlace de expresiones, y el formato que definen la apariencia y la funcionalidad del formulario.</span><span class="sxs-lookup"><span data-stu-id="78f53-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="78f53-190">Conectar el marcado anterior a la base de datos requiere código adicional.</span><span class="sxs-lookup"><span data-stu-id="78f53-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="78f53-191">En **el Explorador de soluciones**, haga clic en *ProductDetails.aspx* y, a continuación, seleccione **ver código**.</span><span class="sxs-lookup"><span data-stu-id="78f53-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then select **View Code**.</span></span>  
   <span data-ttu-id="78f53-192">El *ProductDetails.aspx.cs* se muestra el archivo.</span><span class="sxs-lookup"><span data-stu-id="78f53-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="78f53-193">Reemplace el código existente con este:</span><span class="sxs-lookup"><span data-stu-id="78f53-193">Replace the existing code with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="78f53-194">Este código comprueba si hay un "`productID`" valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="78f53-194">This code checks for a "`productID`" query string value.</span></span> <span data-ttu-id="78f53-195">Si se encuentra un valor válido, se muestra el producto coincidente.</span><span class="sxs-lookup"><span data-stu-id="78f53-195">If a valid value is found, the matching product is displayed.</span></span> <span data-ttu-id="78f53-196">Si no se encuentra la cadena de consulta o su valor no es válido, no se muestra ningún producto.</span><span class="sxs-lookup"><span data-stu-id="78f53-196">If the query string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="78f53-197">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="78f53-197">Run the application</span></span>

<span data-ttu-id="78f53-198">Ahora puede ejecutar la aplicación para ver los detalles de producto específico según el identificador de producto.</span><span class="sxs-lookup"><span data-stu-id="78f53-198">Now you can run the application to see specific product details based on product ID.</span></span>

1. <span data-ttu-id="78f53-199">En Visual Studio, presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="78f53-199">In Visual Studio, press **F5** to run the application.</span></span>  
 <span data-ttu-id="78f53-200">El explorador se abre en *Default.aspx*.</span><span class="sxs-lookup"><span data-stu-id="78f53-200">The browser opens to *Default.aspx*.</span></span>

2. <span data-ttu-id="78f53-201">En el menú de la categoría, seleccione **barcos**.</span><span class="sxs-lookup"><span data-stu-id="78f53-201">From the category menu, select **Boats**.</span></span>  
 <span data-ttu-id="78f53-202">El *ProductList.aspx* se muestra la página.</span><span class="sxs-lookup"><span data-stu-id="78f53-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="78f53-203">Seleccione **papel barco**.</span><span class="sxs-lookup"><span data-stu-id="78f53-203">Select **Paper Boat**.</span></span>  
 <span data-ttu-id="78f53-204">El *ProductDetails.aspx* se muestra la página.</span><span class="sxs-lookup"><span data-stu-id="78f53-204">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Mostrar datos de elementos y detalles - productos](display_data_items_and_details/_static/image4.png)

<span data-ttu-id="78f53-206">En el siguiente tutorial, agregará un carro de la compra a la aplicación Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="78f53-206">In the next tutorial, you add a shopping cart to the Wingtip Toys application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="78f53-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="78f53-207">Additional resources</span></span>

[<span data-ttu-id="78f53-208">Recuperar y mostrar datos con enlace de modelos y formularios web forms</span><span class="sxs-lookup"><span data-stu-id="78f53-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="78f53-209">[Anterior](ui_and_navigation.md)
> [Siguiente](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="78f53-209">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
