---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: "-Parte 5: Lógica de negocios | Documentos de Microsoft"
author: JoeStagner
description: "Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 5 agrega alguna lógica de negocios."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e205788e05a2ad94d86d4847c11c40898b1c3113
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-5-business-logic"></a><span data-ttu-id="85e48-104">-Parte 5: Lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="85e48-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="85e48-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="85e48-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="85e48-106">Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="85e48-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="85e48-107">Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="85e48-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="85e48-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="85e48-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="85e48-109">Parte 5 agrega alguna lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="85e48-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a><span data-ttu-id="85e48-110">Agregar alguna lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="85e48-110">Adding Some Business Logic</span></span>

<span data-ttu-id="85e48-111">Deseamos que nuestra experiencia compra esté disponible cada vez que alguien visite nuestro sitio web.</span><span class="sxs-lookup"><span data-stu-id="85e48-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="85e48-112">Los visitantes podrán examinar y agregar elementos al carro de la compra, incluso si no están registrados o registrados en.</span><span class="sxs-lookup"><span data-stu-id="85e48-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="85e48-113">Cuando estén listas desproteger se le dará la opción de autenticar y si no lo son todavía los miembros podrán crear una cuenta.</span><span class="sxs-lookup"><span data-stu-id="85e48-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="85e48-114">Esto significa que tendrá que implementar la lógica para convertir el carro de la compra de un estado anónimo a un estado de "Usuario registrado".</span><span class="sxs-lookup"><span data-stu-id="85e48-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="85e48-115">Vamos a crear un directorio denominado "Clases", a continuación, haga doble clic en la carpeta y cree un nuevo archivo de "Clase" denominado MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="85e48-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="85e48-116">Como se mencionó anteriormente se tendrá que ampliar la clase que implementa la página de MyShoppingCart.aspx y se encargará de hacerlo con. Construcción de "Clase parcial" eficaz de NET.</span><span class="sxs-lookup"><span data-stu-id="85e48-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="85e48-117">La llamada para nuestro archivo MyShoppingCart.aspx.cf generada tiene el siguiente aspecto.</span><span class="sxs-lookup"><span data-stu-id="85e48-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="85e48-118">Tenga en cuenta el uso de la palabra clave "partial".</span><span class="sxs-lookup"><span data-stu-id="85e48-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="85e48-119">El archivo de clase que se acaba de generar el siguiente aspecto.</span><span class="sxs-lookup"><span data-stu-id="85e48-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="85e48-120">Agregando la palabra clave partial a este archivo también se combinará nuestras implementaciones.</span><span class="sxs-lookup"><span data-stu-id="85e48-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="85e48-121">Nuestro archivo de clase nuevo ahora este aspecto.</span><span class="sxs-lookup"><span data-stu-id="85e48-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="85e48-122">El primer método que se agregará a la clase es el método de "AddItem".</span><span class="sxs-lookup"><span data-stu-id="85e48-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="85e48-123">Este es el método que se llamará en última instancia cuando el usuario hace clic en los vínculos de "Agregar a imágenes" en las páginas de la lista de productos y detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="85e48-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="85e48-124">Anexe lo siguiente para el uso de instrucciones en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="85e48-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="85e48-125">Y agregue este método a la clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="85e48-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="85e48-126">Estamos utilizando LINQ to Entities para ver si el elemento ya está en el carro.</span><span class="sxs-lookup"><span data-stu-id="85e48-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="85e48-127">Si por lo tanto, se actualiza la cantidad del pedido del elemento, en caso contrario, se crea una nueva entrada para el elemento seleccionado</span><span class="sxs-lookup"><span data-stu-id="85e48-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="85e48-128">Para poder llamar a este método se implementará una página AddToCart.aspx que no solo este método de la clase pero, a continuación, muestra un carro = de la compra cuando se ha agregado el elemento actual.</span><span class="sxs-lookup"><span data-stu-id="85e48-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="85e48-129">Haga doble clic en el nombre de la solución en el Explorador de soluciones y agregue y nueva página denominada AddToCart.aspx como lo hemos hecho previamente.</span><span class="sxs-lookup"><span data-stu-id="85e48-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="85e48-130">Aunque podríamos usar esta página para mostrar los resultados intermedios como problemas de cotizaciones bajos, etcetera, en nuestra implementación, la página se realmente no representan, pero en su lugar llame a la lógica de "Agregar" y redirigir.</span><span class="sxs-lookup"><span data-stu-id="85e48-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="85e48-131">Para ello, vamos a agregar el código siguiente a la página\_evento Load.</span><span class="sxs-lookup"><span data-stu-id="85e48-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="85e48-132">Tenga en cuenta que estamos recuperando el producto que desea agregar al carro de la compra de un parámetro de cadena de consulta y llamar al método AddItem de nuestra clase.</span><span class="sxs-lookup"><span data-stu-id="85e48-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="85e48-133">Suponiendo que no hay errores se encuentran el control pasa a la página de SHoppingCart.aspx que totalmente implementaremos a continuación.</span><span class="sxs-lookup"><span data-stu-id="85e48-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="85e48-134">Si es necesario efectuar un error que se produzca una excepción.</span><span class="sxs-lookup"><span data-stu-id="85e48-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="85e48-135">Actualmente nos hemos no ha implementado un controlador de errores global para esta excepción quedarían no controlada por nuestra aplicación, pero se solucionará esto en breve.</span><span class="sxs-lookup"><span data-stu-id="85e48-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="85e48-136">Tenga en cuenta también el uso de la instrucción Debug.Fail() (disponible a través de`using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="85e48-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="85e48-137">Es la aplicación se ejecuta en el depurador, este método mostrará un cuadro de diálogo detallado con información sobre el estado de las aplicaciones junto con el mensaje de error que se especifique.</span><span class="sxs-lookup"><span data-stu-id="85e48-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="85e48-138">Cuando se ejecuta en producción el Debug.Fail() se omite la instrucción.</span><span class="sxs-lookup"><span data-stu-id="85e48-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="85e48-139">Puede observar en el código por encima de una llamada a un método en nuestros nombres de clase de carro de la compra "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="85e48-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="85e48-140">Agregue el código para implementar el método como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="85e48-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="85e48-141">Tenga en cuenta que también hemos agregado botones Actualizar y desprotección y una etiqueta donde podemos mostrar el carro de la "total".</span><span class="sxs-lookup"><span data-stu-id="85e48-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="85e48-142">Ahora podemos agregar elementos a nuestro carro de la compra, pero no hemos implementado la lógica para mostrar el carro después de que se ha agregado un producto.</span><span class="sxs-lookup"><span data-stu-id="85e48-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="85e48-143">Por lo tanto, en la página MyShoppingCart.aspx vamos a agregar un control EntityDataSource y GridVire como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="85e48-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="85e48-144">Llame el formulario en el diseñador para que pueda haga doble clic en el botón actualizar el carro y generar el controlador de eventos click que se especifica en la declaración en el marcado.</span><span class="sxs-lookup"><span data-stu-id="85e48-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="85e48-145">Implementaremos los detalles más adelante, pero hacerlo Permítanos compilará y ejecutará nuestra aplicación sin errores.</span><span class="sxs-lookup"><span data-stu-id="85e48-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="85e48-146">Cuando se ejecuta la aplicación y agregar un elemento al carro de la compra se verá lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="85e48-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="85e48-147">Tenga en cuenta que nos hemos desviado de la presentación de la cuadrícula de "default" mediante la implementación de tres columnas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="85e48-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="85e48-148">El primero es un Editable, el campo "Enlace" para la cantidad:</span><span class="sxs-lookup"><span data-stu-id="85e48-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="85e48-149">La siguiente es una columna "calculada" que muestra el elemento de línea total (el elemento de coste multiplicado por la cantidad para poder ordenarse):</span><span class="sxs-lookup"><span data-stu-id="85e48-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="85e48-150">Por último, tenemos una columna personalizada que contiene un control de casilla de verificación que el usuario va a emplear para indicar que el elemento debe quitarse del plan de compra.</span><span class="sxs-lookup"><span data-stu-id="85e48-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="85e48-151">Como puede ver, el orden Total de líneas está vacía, así que vamos a agregar lógica para calcular el Total del pedido.</span><span class="sxs-lookup"><span data-stu-id="85e48-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="85e48-152">Implementaremos en primer lugar un método de "GetTotal" a nuestra clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="85e48-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="85e48-153">En el archivo MyShoppingCart.cs agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="85e48-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="85e48-154">A continuación, en la página\_controlador de eventos de carga se le puede llamar a nuestro método GetTotal.</span><span class="sxs-lookup"><span data-stu-id="85e48-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="85e48-155">Al mismo tiempo, vamos a agregar una prueba para comprobar si el carro de la compra está vacío y ajuste la presentación en consecuencia, si.</span><span class="sxs-lookup"><span data-stu-id="85e48-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="85e48-156">Ahora si el carro de la compra está vacío nos enfrentamos a esto:</span><span class="sxs-lookup"><span data-stu-id="85e48-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="85e48-157">Y si no es así, se muestra el total.</span><span class="sxs-lookup"><span data-stu-id="85e48-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="85e48-158">Sin embargo, esta página no está completa.</span><span class="sxs-lookup"><span data-stu-id="85e48-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="85e48-159">Necesitamos lógica adicional para volver a calcular el carro de la compra mediante la eliminación de elementos marcados para su eliminación y mediante la determinación de nuevos valores de cantidad como algunos pueden se han cambiado en la cuadrícula por el usuario.</span><span class="sxs-lookup"><span data-stu-id="85e48-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="85e48-160">Permite agregar un método de "RemoveItem" a nuestra clase de carro de la compra en MyShoppingCart.cs para controlar el caso cuando un usuario marca un elemento para su eliminación.</span><span class="sxs-lookup"><span data-stu-id="85e48-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="85e48-161">Ahora vamos a ad un método para controlar las circunstancias, cuando un usuario simplemente cambia la calidad se ordenen en GridView.</span><span class="sxs-lookup"><span data-stu-id="85e48-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="85e48-162">Podemos implementar la lógica que realmente actualiza el carro de la compra en la base de datos con las características básicas de quitar y actualizar en su lugar.</span><span class="sxs-lookup"><span data-stu-id="85e48-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="85e48-163">(En MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="85e48-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="85e48-164">Se deberá tener en cuenta que este método espera dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="85e48-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="85e48-165">Uno es la cesta de identificador y el otro es una matriz de objetos de tipo definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="85e48-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="85e48-166">Con el fin de minimizar la dependencia de nuestra lógica sobre los detalles de la interfaz de usuario, hemos definido una estructura de datos que podemos usar para pasar los elementos del carro de la compra a nuestro código sin el método que necesitan tener acceso directamente el control GridView.</span><span class="sxs-lookup"><span data-stu-id="85e48-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="85e48-167">En nuestro archivo MyShoppingCart.aspx.cs podemos usar esta estructura en el controlador de evento Click de botón de actualización como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="85e48-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="85e48-168">Tenga en cuenta que además de actualizar el carro actualizaremos también el total de carro.</span><span class="sxs-lookup"><span data-stu-id="85e48-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="85e48-169">Tenga en cuenta con un interés particular esta línea de código:</span><span class="sxs-lookup"><span data-stu-id="85e48-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="85e48-170">GetValues() es una función auxiliar especial que se implementará en MyShoppingCart.aspx.cs como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="85e48-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="85e48-171">Esto proporciona una manera limpia para tener acceso a los valores de los elementos de enlace de nuestro control GridView.</span><span class="sxs-lookup"><span data-stu-id="85e48-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="85e48-172">Puesto que nuestro Control de casilla de verificación "Quitar el elemento" no está enlazado se podrá acceder a él a través del método FindControl().</span><span class="sxs-lookup"><span data-stu-id="85e48-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="85e48-173">En esta fase de desarrollo del proyecto ahora obtenemos está listos para implementar el proceso de pago.</span><span class="sxs-lookup"><span data-stu-id="85e48-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="85e48-174">Antes de que al hacerlo, vamos a usar Visual Studio para generar la base de datos de pertenencia y agregar un usuario en el repositorio de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="85e48-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="85e48-175">[Anterior](tailspin-spyworks-part-4.md)
[Siguiente](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="85e48-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
