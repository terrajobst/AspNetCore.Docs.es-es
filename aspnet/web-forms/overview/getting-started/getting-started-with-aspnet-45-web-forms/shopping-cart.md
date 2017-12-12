---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Carro de la compra | Documentos de Microsoft
author: Erikre
description: "Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 5c0e16df7d60b944c96f8d5510225fff321124d1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="shopping-cart"></a>Carro de la compra
====================
Por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar libros electrónicos (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible como acompañamiento de esta serie de tutoriales.


Este tutorial describe la lógica comercial necesaria para agregar un carro de la compra a la aplicación de formularios Web Forms de ASP.NET de ejemplo de Wingtip Toys. Este tutorial se basa en el tutorial anterior "Mostrar datos de elementos y detalles" y forma parte de la serie de tutoriales de almacén de Wingtip Toys. Cuando haya completado este tutorial, los usuarios de la aplicación de ejemplo podrá agregar, quitar y modificar los productos del carro de la compra.

## <a name="what-youll-learn"></a>Lo que aprenderá:

1. Cómo crear un carro de la compra para la aplicación web.
2. Cómo permitir a los usuarios agregar elementos al carro de la compra.
3. Cómo agregar un [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) control para mostrar los detalles de carro de la compra.
4. Cómo se calculan y muestran el total del pedido.
5. Cómo quitar y actualizar los elementos en el carro de la compra.
6. Cómo incluir un contador de carro de la compra.

## <a name="code-features-in-this-tutorial"></a>Características de código de este tutorial:

1. Entity Framework Code First
2. Anotaciones de datos
3. Establecimiento inflexible de tipos controles de datos
4. Enlace de modelos

## <a name="creating-a-shopping-cart"></a>Creación de un carro de la compra

Anteriormente en esta serie de tutoriales, agrega las páginas y el código para ver datos de producto de una base de datos. En este tutorial, creará un carro de la compra para administrar los productos que los usuarios están interesados en comprar. Los usuarios podrán examinar y agregar elementos al carro de la compra incluso si no están registrados o ha iniciado sesión. Para administrar el acceso de carro de la compra, se asignará a los usuarios un único `ID` utilizando un identificador único global (GUID) cuando el usuario tiene acceso a la compra carro por primera vez. Va a almacenar esto `ID` con el estado de sesión de ASP.NET.

> [!NOTE] 
> 
> El estado de sesión de ASP.NET es un lugar conveniente para almacenar información específica del usuario que expirará después de que el usuario abandona el sitio. Mientras un uso incorrecto del estado de sesión puede tener implicaciones de rendimiento en sitios de mayor tamaño, claro el uso de la sesión de estado funciona bien con fines de demostración. El proyecto de ejemplo de Wingtip Toys muestra cómo usar el estado de sesión sin un proveedor externo, donde el estado de sesión está almacenado en proceso en el servidor web que hospeda el sitio. Para los sitios más grandes que proporcionan varias instancias de una aplicación o para sitios que se ejecutan varias instancias de una aplicación en distintos servidores, considere el uso de **servicio de caché de Windows Azure**. Este servicio de caché proporciona un servicio de almacenamiento en caché distribuido que es externo al sitio web y se resuelve el problema de utilizar el estado de sesión en proceso. Para obtener más información, vea [el estado de sesión de ASP.NET de uso con sitios Web de Windows Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Agregar CartItem como una clase de modelo

Anteriormente en esta serie de tutoriales, ha definido el esquema para los datos de categoría y producto mediante la creación de la `Category` y `Product` clases en el *modelos* carpeta. Ahora, agregue una nueva clase para definir el esquema para el carro de la compra. Más adelante en este tutorial, agregará una clase para controlar el acceso a los datos del `CartItem` tabla. Esta clase proporcionará la lógica de negocios para agregar, quitar y actualizar los elementos en el carro de la compra.

1. Haga clic en el *modelos* carpeta y seleccione **agregar**  - &gt; **nuevo elemento**. 

    ![Carro de la compra - nuevo elemento](shopping-cart/_static/image1.png)
2. Se abrirá el cuadro de diálogo **Agregar nuevo elemento**. Seleccione **código**y, a continuación, seleccione **clase**. 

    ![Carro de la compra - Agregar nuevo elemento de cuadro de diálogo](shopping-cart/_static/image2.png)
3. Esta nueva clase el nombre *CartItem.cs*.
4. Haga clic en **Agregar**.  
 El nuevo archivo de clase se muestra en el editor.
5. Reemplace el código predeterminado por el código siguiente:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

La `CartItem` clase contiene el esquema que define cada producto que un usuario agrega al carro de la compra. Esta clase es similar a las demás clases de esquema que creó anteriormente en esta serie de tutoriales. Por convención, Entity Framework Code First que así lo exige la clave principal de la `CartItem` tabla será `CartItemId` o `ID`. Sin embargo, el código invalida el comportamiento predeterminado mediante la anotación de datos `[Key]` atributo. El `Key` atributo de la propiedad ItemId especifica que el `ItemID` propiedad es la clave principal.

El `CartId` propiedad especifica el `ID` del usuario que está asociado con el artículo que desea adquirir. Agregará código para crear este usuario `ID` cuando el usuario tiene acceso a la cesta. Esto `ID` también se almacenará como una variable de sesión de ASP.NET.

### <a name="update-the-product-context"></a>Actualizar el contexto de producto

Además de agregar el `CartItem` (clase), debe actualizar la clase de contexto de base de datos que administra las clases de entidad y que proporciona acceso a datos en la base de datos. Para ello, agregará el recién creado `CartItem` clase al modelo el `ProductContext` clase.

1. En **el Explorador de soluciones**, busque y abra la *ProductContext.cs* un archivo en el *modelos* carpeta.
2. Agregue el código resaltado a la *ProductContext.cs* como sigue:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Como se mencionó anteriormente en esta serie de tutoriales, el código en el *ProductContext.cs* archivo agrega el `System.Data.Entity` espacio de nombres para que tengan acceso a toda la funcionalidad básica de Entity Framework. Esta funcionalidad incluye la capacidad de consultar, insertar, actualizar y eliminar datos trabajando con objetos fuertemente tipados. El `ProductContext` clase agrega el acceso al recién agregado `CartItem` clase de modelo.

### <a name="managing-the-shopping-cart-business-logic"></a>Administrar la lógica de negocios de carro de la compra

A continuación, creará el `ShoppingCart` clase en una nueva *lógica* carpeta. El `ShoppingCart` clase controla el acceso a los datos del `CartItem` tabla. La clase también incluye la lógica de negocios para agregar, quitar y actualizar los elementos en el carro de la compra.

La lógica de carro de la compra que va a agregar contendrá la funcionalidad para administrar las siguientes acciones:

1. Agregar elementos al carro de la compra
2. Quitar elementos de la cesta
3. Obtener el identificador de la cesta de compra
4. Recuperando elementos del carro de la compra
5. Un total de la cantidad de todos los elementos del carro de la compra
6. Actualizar los datos del carro de la compra

Una página de carro de la compra (*ShoppingCart.aspx*) y la clase de carro de la compra se utilizará juntos para tener acceso a datos del carro de compra. La página de carro de compra mostrará todos los elementos que el usuario agrega al carro de la compra. Además de los artículos al carro de clase y la página, podrá crear una página (*AddToCart.aspx*) para agregar productos a la cesta. También agregará código para el *ProductList.aspx* página y la *ProductDetails.aspx* página que le proporcionará un vínculo a la *AddToCart.aspx* página, por lo que puede agregar el usuario productos del carro de la compra.

El siguiente diagrama muestra el proceso básico que tiene lugar cuando el usuario agrega un producto al carro de la compra.

![Carro de la compra - agregar al carro de la compra](shopping-cart/_static/image3.png)

Cuando el usuario hace clic en el **Add To Cart** vínculo cualquiera el *ProductList.aspx* página o *ProductDetails.aspx* página, la aplicación se le remitirá a la *AddToCart.aspx* página y, a continuación, automáticamente a la *ShoppingCart.aspx* página. El *AddToCart.aspx* página agregará el producto seleccione al carro de la compra mediante una llamada a un método en la clase ShoppingCart. El *ShoppingCart.aspx* página mostrará los productos que se han agregado al carro de la compra.

#### <a name="creating-the-shopping-cart-class"></a>Crear la clase de carro de la compra

La `ShoppingCart` clase se agregará a una carpeta independiente en la aplicación de modo que será una distinción clara entre el modelo (carpeta Models), las páginas (carpeta raíz) y la lógica (carpeta lógica).

1. En **el Explorador de soluciones**, haga clic en el **WingtipToys**de proyecto y seleccione **agregar**-&gt;**nueva carpeta**. Nombre de la nueva carpeta *lógica*.
2. Haga clic en el *lógica* carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**.
3. Agregar un nuevo archivo de clase denominado *ShoppingCartActions.cs*.
4. Reemplace el código predeterminado por el código siguiente:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

El `AddToCart` método permite que los productos individuales que se incluirá en el carro de la compra según el producto `ID`. El producto se agrega al carro de la, o si el carro ya contiene un elemento correspondiente a ese producto, la cantidad se incrementa.

El `GetCartId` método devuelve el carro `ID` para el usuario. El carro `ID` se utiliza para realizar un seguimiento de los elementos que un usuario tiene en su carro de la compra. Si el usuario no tiene un carro de la existente `ID`, un carro de la nueva `ID` creadas para ellos. Si el usuario ha iniciado sesión como un usuario registrado, el carro `ID` está establecido en su nombre de usuario. Sin embargo, si el usuario no ha iniciado sesión, el carro `ID` se establece en un valor único (GUID). Un GUID se asegura de que solo un carro se crea para cada usuario, basándose en la sesión.

El `GetCartItems` método devuelve una lista de artículos del carro para el usuario de la compra. Más adelante en este tutorial, verá que el enlace de modelos se utiliza para mostrar los elementos de carro de la compra carro mediante el `GetCartItems` método.

### <a name="creating-the-add-to-cart-functionality"></a>Crear la funcionalidad de agregar a la compra

Como se mencionó anteriormente, se creará una página de procesamiento denominada *AddToCart.aspx* que se usará para agregar nuevos productos al carro de la compra del usuario. Llame esta página la `AddToCart` método en la `ShoppingCart` clase que acaba de crear. El *AddToCart.aspx* página esperará a que un producto `ID` se pasa a él. Este producto `ID` se usará cuando se llama a la `AddToCart` método en la `ShoppingCart` clase.

> [!NOTE] 
> 
> Modificará el código subyacente (*AddToCart.aspx.cs*) para esta página, no la página de interfaz de usuario (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Para crear el complemento al carro funcionalidad:

1. En **el Explorador de soluciones**, haga clic en el **WingtipToys**proyecto de equipo y haga clic en **agregar**  - &gt; **nuevo elemento**.  
 Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Agregar una nueva página estándar (formulario Web Forms) a la aplicación denominada *AddToCart.aspx*. 

    ![Carro de la compra - Agregar formulario Web Forms](shopping-cart/_static/image4.png)
3. En **el Explorador de soluciones**, haga clic en el *AddToCart.aspx* página y, a continuación, haga clic en **ver código**. El *AddToCart.aspx.cs* se abre el archivo de código subyacente en el editor.
4. Reemplace el código existente en el *AddToCart.aspx.cs* código subyacente con lo siguiente:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Cuando el *AddToCart.aspx* página se carga, el producto `ID` se recupera de la cadena de consulta. A continuación, se crea y se utiliza para llamar a una instancia de la clase de carro de la compra el `AddToCart` método que agregó anteriormente en este tutorial. El `AddToCart` método, incluido en el *ShoppingCartActions.cs* de archivos, incluye la lógica para agregar el producto seleccionado al carro de la compra o incrementar la cantidad de productos del producto seleccionado. Si el producto no se ha agregado al carro de la compra, el producto se agrega a la `CartItem` tabla de la base de datos. Si el producto ya se ha agregado al carro de la compra y el usuario agrega un elemento adicional del mismo producto, se incrementa la cantidad de productos en la `CartItem` tabla. Por último, la página se redirige a la *ShoppingCart.aspx* página que agregará en el paso siguiente, donde el usuario ve una lista actualizada de los artículos del carro.

Como se mencionó anteriormente, un usuario `ID` se usa para identificar los productos que están asociados a un usuario específico. Esto `ID` se agrega a una fila de la `CartItem` cada vez que el usuario agrega un producto a la cesta de la tabla.

### <a name="creating-the-shopping-cart-ui"></a>Creación de la cesta de la interfaz de usuario

El *ShoppingCart.aspx* página mostrará los productos que el usuario ha agregado a su carro de la compra. También ofrecerá la capacidad de agregar, quitar y actualizar los elementos en el carro de la compra.

1. En **el Explorador de soluciones**, haga clic en **WingtipToys**, haga clic en **agregar**  - &gt; **nuevo elemento**.  
 Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Agregue una nueva página (formulario Web Forms) que incluye una página maestra seleccionando **formulario Web con página maestra**. Nombre de la nueva página *ShoppingCart.aspx*.
3. Seleccione **Site.Master** para adjuntar la página maestra a recién creado *.aspx* página.
4. En el *ShoppingCart.aspx* página, reemplace el marcado existente con el marcado siguiente:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

El *ShoppingCart.aspx* página incluye una **GridView** control denominado `CartList`. Este control usa el enlace de modelos para enlazar los datos del carro de la compra de la base de datos a la **GridView** control. Al establecer el `ItemType` propiedad de la **GridView** controlar, la expresión de enlace de datos `Item` está disponible en el marcado del control y el control pasa a ser fuertemente tipado. Como se mencionó anteriormente en esta serie de tutoriales, puede seleccionar los detalles de la `Item` objeto mediante IntelliSense. Para configurar un control de datos para utilizar el enlace de modelos para seleccionar datos, debe establecer el `SelectMethod` propiedad del control. En el marcado, establezca la `SelectMethod` utilizar el método GetShoppingCartItems que devuelve una lista de `CartItem` objetos. El **GridView** control de datos llama al método en el momento adecuado en el ciclo de vida de la página y se enlaza automáticamente los datos devueltos. El `GetShoppingCartItems` método todavía debe agregarse.

#### <a name="retrieving-the-shopping-cart-items"></a>Recuperar los elementos del carro de la compra

A continuación, agregue código a la *ShoppingCart.aspx.cs* código subyacente para recuperar y rellenar la interfaz de usuario del carro de la compra.

1. En **el Explorador de soluciones**, haga clic en el *ShoppingCart.aspx* página y, a continuación, haga clic en **ver código**. El *ShoppingCart.aspx.cs* se abre el archivo de código subyacente en el editor.
2. Reemplace el código existente por el siguiente:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Como se mencionó anteriormente, el `GridView` llamadas de control de datos el `GetShoppingCartItems` ciclo de método en el momento adecuado en la vida de la página y se enlaza automáticamente los datos devueltos. El `GetShoppingCartItems` método crea una instancia de la `ShoppingCartActions` objeto. A continuación, el código usa esa instancia para devolver los elementos en el carro mediante una llamada a la `GetCartItems` método.

### <a name="adding-products-to-the-shopping-cart"></a>Agregar productos al carro de la compra

Cuando ambos el *ProductList.aspx* o *ProductDetails.aspx* se muestra la página, el usuario podrá agregar el producto a la cesta de compra utilizando un vínculo. Cuando hagan clic en el vínculo, la aplicación navega a la página de procesamiento denominada *AddToCart.aspx*. El *AddToCart.aspx* página llamará el `AddToCart` método en la `ShoppingCart` clase que agregó anteriormente en este tutorial.

Ahora, agregará un **agregar al carro** vínculo a ambos el *ProductList.aspx* página y la *ProductDetails.aspx* página. Este vínculo incluirá el producto `ID` que se recupera de la base de datos.

1. En **el Explorador de soluciones**, busque y abra la página denominada *ProductList.aspx*.
2. Agregue el marcado que se resalta en amarillo en el *ProductList.aspx* página para que toda la página aparece como sigue:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Probar el carro de la compra

Ejecute la aplicación para ver cómo agregar productos a la cesta.

1. Presione **F5** para ejecutar la aplicación.  
 Después de que el proyecto vuelve a crear la base de datos, el explorador se abrirá y mostrar el *Default.aspx* página.
2. Seleccione **automóviles** en el menú de navegación de la categoría.  
 El *ProductList.aspx* página se mostrará solo los productos incluidos en la categoría "Automóvil". 

    ![Carro de la compra - automóviles](shopping-cart/_static/image5.png)
3. Haga clic en el **agregar al carro** vínculo situado junto a la primera producto aparece (el coche convertible).   
 El *ShoppingCart.aspx* aparece la página, que muestra la selección en el carro de la compra. 

    ![Carro - carro de la compra](shopping-cart/_static/image6.png)
4. Ver los productos adicionales, seleccione **planos** en el menú de navegación de la categoría.
5. Haga clic en el **agregar al carro** vínculo situado junto a la primera producto enumerado.  
 El *ShoppingCart.aspx* se muestra la página con el elemento adicional.
6. Cierre el explorador.

### <a name="calculating-and-displaying-the-order-total"></a>Calcular y mostrar el Total del pedido

Además de agregar productos al carro de la compra, agregará un `GetTotal` método a la `ShoppingCart` clase y mostrar la cantidad total del pedido en la página de carro de la compra.

1. En **el Explorador de soluciones**, abra el *ShoppingCartActions.cs* un archivo en el *lógica* carpeta.
2. Agregue el siguiente `GetTotal` método aparecen resaltada en amarillo en el `ShoppingCart` de la clase, por lo que la clase aparezca como sigue:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

En primer lugar, el `GetTotal` método obtiene el identificador del carro de la compra del usuario. A continuación, el método obtiene el carro de la total multiplicando el precio del producto por la cantidad de productos para cada producto enumerado en el carro.

> [!NOTE] 
> 
> El código anterior utiliza el tipo que acepta valores null "`int?`". Tipos que aceptan valores null pueden representar todos los valores de un tipo subyacente así como un valor nulo. Para obtener más información, vea [utilizar tipos que aceptan valores NULL](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Modificar la presentación de carro de la compra

A continuación modificará el código para el *ShoppingCart.aspx* página para llamar a la `GetTotal` método y mostrar total en el *ShoppingCart.aspx* página cuando se carga la página.

1. En **el Explorador de soluciones**, haga clic en el *ShoppingCart.aspx* página y seleccione **ver código**.
2. En el *ShoppingCart.aspx.cs* de archivos, actualice el `Page_Load` controlador agregando el código siguiente se resaltan en amarillo:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Cuando el *ShoppingCart.aspx* carga de página, se carga el objeto de carro de la compra y, a continuación, recupera el total de carro de la compra mediante una llamada a la `GetTotal` método de la `ShoppingCart` clase. Si el carro de la compra está vacío, a tal efecto se muestra un mensaje.

### <a name="testing-the-shopping-cart-total"></a>Probar el Total de carro de la compra

Ejecute la aplicación para ver cómo no solo puede agregar un producto al carro de la compra, pero puede ver el total de carro de la compra.

1. Presione **F5** para ejecutar la aplicación.  
 El explorador se abrirá y mostrar el *Default.aspx* página.
2. Seleccione **automóviles** en el menú de navegación de la categoría.
3. Haga clic en el **Add To Cart** vínculo situado junto a la primera el producto.   
 El *ShoppingCart.aspx* página se muestra con el total del pedido. 

    ![Shopping Cart - Total de carro](shopping-cart/_static/image7.png)
4. Agregar otros productos (por ejemplo, un plano) al carro.
5. El *ShoppingCart.aspx* página se muestra con un total actualizado para todos los productos que ha agregado. 

    ![Carro de la compra - varios productos](shopping-cart/_static/image8.png)
6. Detener la aplicación en ejecución, el cierre de la ventana del explorador.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Agregar botones de desprotección y actualización al carro de la compra

Para permitir que los usuarios modificar el carro de la compra, agregará un **actualización** botón y un **desprotección** botón hasta la página de carro de la compra. El **desprotección** botón no se usa hasta más adelante en esta serie de tutoriales.

1. En **el Explorador de soluciones**, abra el *ShoppingCart.aspx* página en la raíz del proyecto de aplicación web.
2. Para agregar el **actualización** botón y **desprotección** botón a la *ShoppingCart.aspx* página, agregue el marcado que se resalta en amarillo en el marcado existente, como se muestra en el código siguiente:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Cuando el usuario hace clic en el **actualización** botón, el `UpdateBtn_Click` se llamará el controlador de eventos. Este controlador de eventos llamará el código que agregará en el paso siguiente.

A continuación, puede actualizar el código incluido en el *ShoppingCart.aspx.cs* archivo recorrer los elementos de carro y llamada la `RemoveItem` y `UpdateItem` métodos.

1. En **el Explorador de soluciones**, abra el *ShoppingCart.aspx.cs* archivo en la raíz del proyecto de aplicación web.
2. Agregue las siguientes secciones de código aparecen resaltadas en amarillo en el *ShoppingCart.aspx.cs* archivo:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Cuando el usuario hace clic en el **actualización** situado en la *ShoppingCart.aspx* página, que se llama al método UpdateCartItems. El método UpdateCartItems obtiene los valores actualizados de cada elemento en el carro de la compra. A continuación, llama al método UpdateCartItems el `UpdateShoppingCartDatabase` (método, agrega y explica en el paso siguiente) para agregar o quitar elementos del carro de la compra. Una vez que la base de datos se ha actualizado para reflejar las actualizaciones al carro de la compra, la **GridView** control se actualiza en la página de carro de compra mediante una llamada a la `DataBind` método para el **GridView**. Además, la cantidad total del pedido en la página de carro de la compra se actualiza para reflejar la lista actualizada de elementos.

### <a name="updating-and-removing-shopping-cart-items"></a>Actualización y eliminación de artículos del carro de la compra

En el *ShoppingCart.aspx* página, puede ver los controles se han agregado para la cantidad de un elemento de actualización y eliminación de un elemento. Ahora, agregue el código que hará que estos controles de trabajo.

1. En **el Explorador de soluciones**, abra el *ShoppingCartActions.cs* un archivo en el *lógica* carpeta.
2. Agregue el código siguiente aparecen resaltado en amarillo en el *ShoppingCartActions.cs* archivo de clase:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

El `UpdateShoppingCartDatabase` método, llamado desde el `UpdateCartItems` método en el *ShoppingCart.aspx.cs* página, contiene la lógica para actualizar o quitar elementos de la cesta. El `UpdateShoppingCartDatabase` método recorre en iteración todas las filas en la lista de carro de la compra. Si un elemento del carro de la compra se ha marcado para quitarse o la cantidad es menor que uno, el `RemoveItem` se llama al método. En caso contrario, se comprueba el elemento de carro de la compra se actualiza cuando el `UpdateItem` se llama al método. Después de que se ha quitado o actualizar el elemento de carro de la compra, se guardan los cambios de la base de datos.

El `ShoppingCartUpdates` estructura se usa para contener todos los elementos del carro de la compra. El `UpdateShoppingCartDatabase` método usa la `ShoppingCartUpdates` estructura para determinar si cualquiera de los elementos necesita se actualizó o se quitó.

En el siguiente tutorial, utilizará el `EmptyCart` método para borrar la compra carro después de la compra de productos. Pero por ahora, va a usar el `GetCount` método que acaba de agregar a la *ShoppingCartActions.cs* archivo para determinar cuántos elementos se encuentran en el carro de la compra.

### <a name="adding-a-shopping-cart-counter"></a>Agregar un contador de carro de la compra

Para permitir que el usuario pueda ver el número total de elementos en el carro de la compra, va a agregar un contador a la *Site.Master* página. Este contador también actuará como un vínculo al carro de la compra.

1. En **el Explorador de soluciones**, abra el *Site.Master* página.
2. Modifique el marcado agregando el vínculo de contador de carro de la compra, como se muestra en amarillo en la sección de navegación, por lo que aparece como sigue:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. A continuación, actualice el código subyacente de la *Site.Master.cs* archivo agregando el código que se resalta en amarillo, como se indica a continuación:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Antes de la página se representa como HTML, el `Page_PreRender` evento se desencadena. En el `Page_PreRender` controlador, el número total de carro de la compra se determina mediante una llamada a la `GetCount` método. El valor devuelto se agrega a la `cartCount` intervalo incluido en el marcado de la *Site.Master* página. El `<span>` etiquetas permite a los elementos internos se representen correctamente. Cuando se muestra cualquier página del sitio, se mostrará el total de carro de la compra. El usuario puede hacer clic en el total de carro de la compra para mostrar el carro de la compra.

## <a name="testing-the-completed-shopping-cart"></a>Probar el carro de la compra completado

Puede ejecutar la aplicación ahora para ver cómo se pueden agregar, eliminar y actualiza los artículos en el carro de la compra. El total de carro de la compra reflejará el costo total de todos los elementos en el carro de la compra.

1. Presione **F5** para ejecutar la aplicación.  
 El explorador se abre y muestra el *Default.aspx* página.
2. Seleccione **automóviles** en el menú de navegación de la categoría.
3. Haga clic en el **Add To Cart** vínculo situado junto a la primera el producto.   
 El *ShoppingCart.aspx* página se muestra con el total del pedido.
4. Seleccione **planos** en el menú de navegación de la categoría.
5. Haga clic en el **Add To Cart** vínculo situado junto a la primera el producto.
6. Establecer la cantidad del primer elemento de la cesta a 3 y seleccione la **quitar elemento** casilla de verificación del segundo elemento.<a id="a"></a>
7. Haga clic en el **actualizar** botón para actualizar la página de carro de la compra y mostrar el nuevo total del pedido. 

    ![Carro de la compra - carro de la actualización](shopping-cart/_static/image9.png)

## <a name="summary"></a>Resumen

En este tutorial, ha creado un carro de la compra para la aplicación de ejemplo de formularios Web Forms de Wingtip Toys. Durante este tutorial ha utilizado Entity Framework Code First, las anotaciones de datos, los controles de datos fuertemente tipados y enlace de modelos.

Carro de la compra admite agregar, eliminar y actualizar los elementos que el usuario ha seleccionado para la compra. Además de implementar la funcionalidad de carro de la compra, ha aprendido cómo mostrar los elementos de carro de la compra de un **GridView** controlar y calcular el total del pedido.

## <a name="addition-information"></a>Obtener información adicional

[Información general del estado de sesión de ASP.NET](https://msdn.microsoft.com/en-us/library/ms178581.aspx)

>[!div class="step-by-step"]
[Anterior](display_data_items_and_details.md)
[Siguiente](checkout-and-payment-with-paypal.md)
