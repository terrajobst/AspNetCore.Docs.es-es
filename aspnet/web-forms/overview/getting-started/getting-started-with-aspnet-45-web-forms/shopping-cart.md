---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Carro de la compra | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 22253b8708efd9ce505c9fbeb9cb2e942588b37d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375783"
---
<a name="shopping-cart"></a>Carro de la compra
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible para acompañar esta serie de tutoriales.


Este tutorial describe la lógica de negocios necesaria para agregar un carro de la compra a la aplicación de formularios Web Forms de ASP.NET de ejemplo Wingtip Toys. Este tutorial se basa en el tutorial anterior, "Mostrar datos de elementos y detalles" y forma parte de la serie de tutoriales de Wingtip Toys Store. Cuando haya completado este tutorial, los usuarios de la aplicación de ejemplo podrá agregar, quitar y modificar los productos de su carro de la compra.

## <a name="what-youll-learn"></a>Lo que aprenderá:

1. Cómo crear un carro de la aplicación web.
2. Cómo habilitar usuarios agregar elementos al carro de la compra.
3. Cómo agregar un [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) control para mostrar los detalles de carro de la compra.
4. Cómo se calculan y muestran el total del pedido.
5. Cómo quitar y actualizar los elementos en el carro de la compra.
6. Cómo incluir un contador del carro de la compra.

## <a name="code-features-in-this-tutorial"></a>Funciones de código de este tutorial:

1. Entity Framework Code First
2. Anotaciones de datos
3. Controles de datos fuertemente tipados
4. Enlace de modelos

## <a name="creating-a-shopping-cart"></a>Creación de un carro de la compra

Anteriormente en esta serie de tutoriales, agrega las páginas y el código para ver los datos de producto de una base de datos. En este tutorial, creará un carro de la compra para administrar los productos que los usuarios están interesados en comprar. Los usuarios podrán examinar y agregar elementos al carro de la compra incluso si no están registrados ni ha iniciado sesión. Para administrar el acceso de carro de la compra, se asignará a los usuarios un único `ID` utilizando un identificador único global (GUID) cuando el usuario tiene acceso a la compra el carro por primera vez. Almacenaremos esto `ID` con el estado de sesión de ASP.NET.

> [!NOTE] 
> 
> El estado de sesión de ASP.NET es un lugar conveniente para almacenar información específica del usuario que expirará después de que el usuario abandone el sitio. Mientras un uso incorrecto del estado de sesión puede tener implicaciones de rendimiento en los sitios más grandes, claro de uso de la sesión de estado funciona bien para fines de demostración. El proyecto de ejemplo Wingtip Toys muestra cómo utilizar el estado de sesión sin un proveedor externo, donde el estado de sesión está almacenado en proceso en el servidor web que hospeda el sitio. Para los sitios más grandes que proporcionan varias instancias de una aplicación o para sitios que ejecutan varias instancias de una aplicación en distintos servidores, considere el uso de **Windows Azure Cache Service**. Este servicio de caché proporciona un servicio de almacenamiento en caché distribuido que es externa al sitio web y se resuelve el problema del uso de estado de sesión en proceso. Para obtener más información, vea [cómo el estado de sesión de ASP.NET con sitios Web de Windows Azure de uso](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Agregar CartItem como una clase de modelo

Anteriormente en esta serie de tutoriales, define el esquema de los datos de categoría y producto mediante la creación del `Category` y `Product` clases en el *modelos* carpeta. Ahora, agregue una nueva clase para definir el esquema para el carro de la compra. Más adelante en este tutorial, agregará una clase para controlar el acceso a datos de la `CartItem` tabla. Esta clase proporcionará la lógica de negocios para agregar, quitar y actualizar los elementos en el carro de la compra.

1. Haga clic en el *modelos* carpeta y seleccione **agregar**  - &gt; **nuevo elemento**. 

    ![Carro de compra - nuevo elemento](shopping-cart/_static/image1.png)
2. Se abrirá el cuadro de diálogo **Agregar nuevo elemento**. Seleccione **código**y, a continuación, seleccione **clase**. 

    ![Carro de la compra - Agregar nuevo elemento de cuadro de diálogo](shopping-cart/_static/image2.png)
3. Esta nueva clase el nombre *CartItem.cs*.
4. Haga clic en **Agregar**.  
   El nuevo archivo de clase se muestra en el editor.
5. Reemplace el código predeterminado con el código siguiente:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

La `CartItem` clase contiene el esquema que se van a definir cada producto que un usuario se agrega al carro de la compra. Esta clase es similar a las otras clases de esquema que creó anteriormente en esta serie de tutoriales. Por convención, Entity Framework Code First que así lo exige la clave principal de la `CartItem` tabla será `CartItemId` o `ID`. Sin embargo, el código invalida el comportamiento predeterminado mediante la anotación de datos `[Key]` atributo. El `Key` atributo de la propiedad ItemId especifica que el `ItemID` propiedad es la clave principal.

El `CartId` propiedad especifica el `ID` del usuario que está asociado con el artículo que desea adquirir. Debe agregar código para crear este usuario `ID` cuando el usuario obtiene acceso a la cesta. Esto `ID` también se almacenará como una variable de sesión de ASP.NET.

### <a name="update-the-product-context"></a>Actualizar el contexto de producto

Además de agregar el `CartItem` (clase), deberá actualizar la clase de contexto de base de datos que administra las clases de entidad y que proporciona acceso a datos en la base de datos. Para ello, se agregará el recién creado `CartItem` clase al modelo el `ProductContext` clase.

1. En **el Explorador de soluciones**, busque y abra el *ProductContext.cs* de archivos en el *modelos* carpeta.
2. Agregue el código resaltado a la *ProductContext.cs* archivo como sigue:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Como se mencionó anteriormente en esta serie de tutoriales, el código en el *ProductContext.cs* archivo agrega el `System.Data.Entity` espacio de nombres para que tengan acceso a toda la funcionalidad núcleo de Entity Framework. Esta funcionalidad incluye la capacidad de consultar, insertar, actualizar y eliminar datos trabajando con objetos fuertemente tipados. El `ProductContext` clase agrega acceso recién agregada `CartItem` clase de modelo.

### <a name="managing-the-shopping-cart-business-logic"></a>Administrar la lógica de negocios del carro de la compra

A continuación, creará el `ShoppingCart` clase en un nuevo *lógica* carpeta. El `ShoppingCart` clase controla el acceso a datos de la `CartItem` tabla. La clase también incluirá la lógica de negocios para agregar, quitar y actualizar los elementos en el carro de la compra.

La lógica de carro de la compra que va a agregar contendrá la funcionalidad para administrar las siguientes acciones:

1. Agregar elementos al carro de la compra
2. Quitar elementos del carro de compra
3. Obtener el identificador de la cesta de compra
4. Recuperando elementos del carro de la compra
5. Con un total de la cantidad de todos los elementos del carro de la compra
6. Actualizar los datos del carro de la compra

Una página del carro de compras (*ShoppingCart.aspx*) y la clase del carro de la utilizará juntos para tener acceso a datos del carro de la compra. La página del carro de la compra mostrará todos los elementos que el usuario se agrega al carro de la compra. Además de los artículos al carro de clase y la página, podrá crear una página (*AddToCart.aspx*) para agregar productos al carro de la compra. También agregará código para el *ProductList.aspx* página y el *ProductDetails.aspx* página que proporcionará un vínculo a la *AddToCart.aspx* página, por lo que puede agregar el usuario productos al carro de la compra.

El siguiente diagrama muestra el proceso básico que se produce cuando el usuario agrega un producto al carro de la compra.

![Carro de compra - agregar al carro de la compra](shopping-cart/_static/image3.png)

Cuando el usuario hace clic en el **Add To Cart** vínculo en uno el *ProductList.aspx* página o el *ProductDetails.aspx* página, la aplicación irá a la *AddToCart.aspx* página y, a continuación automáticamente a la *ShoppingCart.aspx* página. El *AddToCart.aspx* página agregará seleccione producto al carro de la compra mediante una llamada a un método en la clase ShoppingCart. El *ShoppingCart.aspx* mostrará la página de los productos que se han agregado al carro de la compra.

#### <a name="creating-the-shopping-cart-class"></a>Creación de la clase del carro de la

La `ShoppingCart` clase se agregará a una carpeta independiente en la aplicación de modo que será una distinción clara entre el modelo (carpeta de modelos), las páginas (carpeta raíz) y la lógica (carpeta lógica).

1. En **el Explorador de soluciones**, haga clic en el **WingtipToys**del proyecto y seleccione **agregar**-&gt;**nueva carpeta**. Nombre de la nueva carpeta *lógica*.
2. Haga clic en el *lógica* carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**.
3. Agregue un nuevo archivo de clase denominado *ShoppingCartActions.cs*.
4. Reemplace el código predeterminado con el código siguiente:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

El `AddToCart` método permite que los productos individuales que se incluirán en el carro de compra basado en el producto `ID`. El producto se agrega al carro de compra, o si el carro de compra ya contiene un elemento para ese producto, se incrementa la cantidad.

El `GetCartId` método devuelve el carro de compra `ID` para el usuario. El carro de compra `ID` se usa para realizar un seguimiento de los elementos que un usuario tiene en su carro de la compra. Si el usuario no tiene un carro existente `ID`, un carro de la nueva `ID` está creado para ellos. Si el usuario ha iniciado sesión como usuario registrado, el carro de compra `ID` está establecido en su nombre de usuario. Sin embargo, si el usuario no ha iniciado sesión, el carro de compra `ID` está establecido en un valor único (GUID). Un GUID garantiza que solo un carro de compra se crea para cada usuario, basado en sesión.

El `GetCartItems` método devuelve una lista de elementos para el usuario del carro de la compra. Más adelante en este tutorial, verá que el enlace de modelos se utiliza para mostrar los elementos del carro de compras carro de compra utilizando el `GetCartItems` método.

### <a name="creating-the-add-to-cart-functionality"></a>Creación de la funcionalidad de agregar al carro

Como se mencionó anteriormente, creará una página de procesamiento denominada *AddToCart.aspx* que se usará para agregar nuevos productos al carro de la compra del usuario. Esta página se llamará el `AddToCart` método en el `ShoppingCart` clase que acaba de crear. El *AddToCart.aspx* página esperará a que un producto `ID` se pasa a él. Este producto `ID` se usará cuando se llama a la `AddToCart` método en el `ShoppingCart` clase.

> [!NOTE] 
> 
> Se modifica el código subyacente (*AddToCart.aspx.cs*) para esta página, no la interfaz de usuario de página (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Para crear el Add To Cart funcionalidad:

1. En **el Explorador de soluciones**, haga clic en el **WingtipToys**del proyecto, haga clic en **agregar**  - &gt; **nuevo elemento**.  
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Agregar una nueva página estándar (formulario Web Forms) a la aplicación denominada *AddToCart.aspx*. 

    ![Carro de la compra: Agregar formulario Web Forms](shopping-cart/_static/image4.png)
3. En **el Explorador de soluciones**, haga clic en el *AddToCart.aspx* página y, a continuación, haga clic en **ver código**. El *AddToCart.aspx.cs* se abre el archivo de código subyacente en el editor.
4. Reemplace el código existente en el *AddToCart.aspx.cs* código subyacente con lo siguiente:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Cuando el *AddToCart.aspx* página se carga, el producto `ID` se recupera de la cadena de consulta. A continuación, se crea y se usa para llamar a una instancia de la clase del carro de la la `AddToCart` método que agregó anteriormente en este tutorial. El `AddToCart` método, incluido en el *ShoppingCartActions.cs* de archivos, que incluye la lógica para agregar el producto seleccionado al carro de la compra o incrementar la cantidad de producto del producto seleccionado. Si el producto no se ha agregado al carro de la compra, el producto se agrega a la `CartItem` tabla de la base de datos. Si el producto ya se agregó al carro de la compra y el usuario agrega un elemento adicional del mismo producto, se incrementa la cantidad de productos en el `CartItem` tabla. Por último, la página se redirige a la *ShoppingCart.aspx* página que agregará en el paso siguiente, donde el usuario ve una lista actualizada de los artículos del carro.

Como se mencionó anteriormente, un usuario `ID` se usa para identificar los productos que están asociados con un usuario específico. Esto `ID` se agrega a una fila de la `CartItem` tabla cada vez que el usuario agrega un producto al carro de la compra.

### <a name="creating-the-shopping-cart-ui"></a>Creación de la interfaz de usuario del carro de compra

El *ShoppingCart.aspx* mostrará la página de los productos que el usuario ha agregado a la cesta. También proporcionará la capacidad de agregar, quitar y actualizar los elementos en el carro de la compra.

1. En **el Explorador de soluciones**, haga clic en **WingtipToys**, haga clic en **agregar**  - &gt; **nuevo elemento**.  
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Agregue una nueva página (formulario Web Forms) que incluye una página maestra seleccionando **formulario Web con página maestra**. Nombre de la nueva página *ShoppingCart.aspx*.
3. Seleccione **Site.Master** para asociar la página maestra recién creado *.aspx* página.
4. En el *ShoppingCart.aspx* página, reemplace el marcado existente por el siguiente marcado:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

El *ShoppingCart.aspx* página incluye un **GridView** control denominado `CartList`. Este control usa el enlace de modelos para enlazar los datos del carro de compras desde la base de datos la **GridView** control. Al establecer el `ItemType` propiedad de la **GridView** controlar, la expresión de enlace de datos `Item` está disponible en el marcado del control y el control pasa a ser inflexible de tipos. Como se mencionó anteriormente en esta serie de tutoriales, puede seleccionar los detalles de la `Item` objeto mediante IntelliSense. Para configurar un control de datos para utilizar el enlace de modelos para seleccionar datos, debe establecer el `SelectMethod` propiedad del control. En el marcado anterior, ha establecido la `SelectMethod` para usar el método GetShoppingCartItems que devuelve una lista de `CartItem` objetos. El **GridView** control de datos llama al método en el momento adecuado en el ciclo de vida de página y se enlaza automáticamente los datos devueltos. El `GetShoppingCartItems` método todavía debe agregarse.

#### <a name="retrieving-the-shopping-cart-items"></a>Recuperar los elementos del carro de la compra

A continuación, agregue código para el *ShoppingCart.aspx.cs* código subyacente para recuperar y rellenar la interfaz de usuario del carro de la compra.

1. En **el Explorador de soluciones**, haga clic en el *ShoppingCart.aspx* página y, a continuación, haga clic en **ver código**. El *ShoppingCart.aspx.cs* se abre el archivo de código subyacente en el editor.
2. Reemplace el código existente por el siguiente:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Como se mencionó anteriormente, el `GridView` las llamadas de control de datos la `GetShoppingCartItems` ciclo de método en el momento adecuado en la vida de la página y se enlaza automáticamente los datos devueltos. El `GetShoppingCartItems` método crea una instancia de la `ShoppingCartActions` objeto. A continuación, el código usa esa instancia para devolver los elementos del carro de compra mediante una llamada a la `GetCartItems` método.

### <a name="adding-products-to-the-shopping-cart"></a>Agregar productos al carro de la compra

Cuando ya sea el *ProductList.aspx* o *ProductDetails.aspx* se muestra la página, el usuario podrá agregar producto al carro de la compra mediante un vínculo. Al hacer clic en el vínculo, la aplicación navega a la página de proceso denominada *AddToCart.aspx*. El *AddToCart.aspx* página llamará el `AddToCart` método en el `ShoppingCart` clase que agregó anteriormente en este tutorial.

Ahora, agregará un **agregar al carro** vínculo a ambos el *ProductList.aspx* página y el *ProductDetails.aspx* página. Este vínculo incluirá el producto `ID` que se recupera de la base de datos.

1. En **el Explorador de soluciones**, busque y abra la página llamada *ProductList.aspx*.
2. Agregue el marcado resaltado en amarillo en la *ProductList.aspx* página para que toda la página aparece como sigue:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Probar el carro de la compra

Ejecute la aplicación para ver cómo agregar productos a la cesta.

1. Presione **F5** para ejecutar la aplicación.  
 Después de que el proyecto se vuelve a crear la base de datos, el explorador se abrirá y mostrará el *Default.aspx* página.
2. Seleccione **automóviles** en el menú de navegación de la categoría.  
 El *ProductList.aspx* se muestra la página que muestra solo los productos incluidos en la categoría "Automóviles". 

    ![Carro de compra - automóviles](shopping-cart/_static/image5.png)
3. Haga clic en el **agregar al carro** vínculo situado junto al primer producto aparece (el automóvil convertible).   
 El *ShoppingCart.aspx* página se muestra, que muestra la selección en su carro de la compra. 

    ![Carro de compra - carro de compra](shopping-cart/_static/image6.png)
4. Ver productos adicionales seleccionando **planos** en el menú de navegación de la categoría.
5. Haga clic en el **agregar al carro** vínculo situado junto a la primera producto enumerado.  
 El *ShoppingCart.aspx* página se muestra con el elemento adicional.
6. Cierre el explorador.

### <a name="calculating-and-displaying-the-order-total"></a>Calcular y mostrar el Total del pedido

Además de agregar productos al carro de la compra, agregará un `GetTotal` método a la `ShoppingCart` clase y mostrar la cantidad total del pedido en la página del carro de la compra.

1. En **el Explorador de soluciones**, abra el *ShoppingCartActions.cs* de archivos en el *lógica* carpeta.
2. Agregue el siguiente `GetTotal` método resaltado en amarillo en la `ShoppingCart` clase, por lo que la clase aparezca como sigue:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

En primer lugar, el `GetTotal` método obtiene el identificador del carro de la compra del usuario. A continuación, el método obtiene el carro de compra total multiplicando el precio del producto por la cantidad de productos para cada producto enumerado en el carro de compra.

> [!NOTE] 
> 
> El código anterior usa el tipo que acepta valores null "`int?`". Tipos que aceptan valores null pueden representar todos los valores de un tipo subyacente y también como un valor null. Para obtener más información, vea [utilizar tipos que aceptan valores NULL](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Modificar la presentación del carro de la compra

A continuación modificará el código para el *ShoppingCart.aspx* página para llamar a la `GetTotal` método y visualización que totales en el *ShoppingCart.aspx* página cuando se carga la página.

1. En **el Explorador de soluciones**, haga clic en el *ShoppingCart.aspx* página y seleccione **ver código**.
2. En el *ShoppingCart.aspx.cs* de archivos, actualice el `Page_Load` controlador agregando el siguiente código resaltado en amarillo:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Cuando el *ShoppingCart.aspx* cargas de página, se carga el objeto de carro de la compra y, a continuación, recupera el total del carro de la compra mediante una llamada a la `GetTotal` método de la `ShoppingCart` clase. Si el carro de la compra está vacía, a tal efecto se muestra un mensaje.

### <a name="testing-the-shopping-cart-total"></a>Probar el Total del carro de la compra

Ejecute ahora la aplicación para ver cómo no solo puede agregar un producto al carro de la compra, pero puede ver el total del carro de la compra.

1. Presione **F5** para ejecutar la aplicación.  
 El explorador se abrirá y mostrará el *Default.aspx* página.
2. Seleccione **automóviles** en el menú de navegación de la categoría.
3. Haga clic en el **Add To Cart** vínculo situado junto al primer producto.   
 El *ShoppingCart.aspx* página se muestra con el total del pedido. 

    ![Carro de la compra - Total de carro](shopping-cart/_static/image7.png)
4. Agregar otros productos (por ejemplo, un plano) al carro de compra.
5. El *ShoppingCart.aspx* página se muestra con un total para todos los productos que ha agregado actualizado. 

    ![Carro de compra - varios productos](shopping-cart/_static/image8.png)
6. Detener la aplicación en ejecución al cerrar la ventana del explorador.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Agregar actualización y botones de desprotección al carro de la compra

Para permitir que los usuarios modificar el carro de la compra, agregará un **actualización** botón y un **desprotección** botón a la página del carro de la compra. El **desprotección** botón no se usa hasta más adelante en esta serie de tutoriales.

1. En **el Explorador de soluciones**, abra el *ShoppingCart.aspx* página en la raíz del proyecto de aplicación web.
2. Para agregar la **actualización** botón y el **desprotección** botón a la *ShoppingCart.aspx* página, agregue el marcado resaltado en amarillo en el marcado existente, como se muestra en el código siguiente:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Cuando el usuario hace clic en el **actualización** botón, el `UpdateBtn_Click` se llamará el controlador de eventos. El código que agregará en el paso siguiente llamará a este controlador de eventos.

A continuación, puede actualizar el código incluido en el *ShoppingCart.aspx.cs* archivo para recorrer los elementos del carro y llamada la `RemoveItem` y `UpdateItem` métodos.

1. En **el Explorador de soluciones**, abra el *ShoppingCart.aspx.cs* archivo en la raíz del proyecto de aplicación web.
2. Agregue las siguientes secciones de código resaltadas en amarillo en la *ShoppingCart.aspx.cs* archivo:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Cuando el usuario hace clic en el **actualización** situado en la *ShoppingCart.aspx* página, en la que se llama al método UpdateCartItems. El método UpdateCartItems obtiene los valores actualizados de cada elemento en el carro de la compra. A continuación, llama al método UpdateCartItems el `UpdateShoppingCartDatabase` (método, agrega y se explica en el paso siguiente) para agregar o quitar elementos del carro de la compra. Una vez que la base de datos se ha actualizado para reflejar las actualizaciones al carro de la compra, la **GridView** control se actualiza en la página del carro de la compra mediante una llamada a la `DataBind` método para el **GridView**. Además, la cantidad total del pedido en la página del carro de la compra se actualiza para reflejar la lista actualizada de los elementos.

### <a name="updating-and-removing-shopping-cart-items"></a>Actualización y eliminación de elementos del carro de la compra

En el *ShoppingCart.aspx* página, puede ver los controles se han agregado para la cantidad de un elemento de actualización y eliminación de un elemento. Ahora, agregue el código que hará que estos controles funcione.

1. En **el Explorador de soluciones**, abra el *ShoppingCartActions.cs* de archivos en el *lógica* carpeta.
2. Agregue el siguiente código resaltado en amarillo en la *ShoppingCartActions.cs* archivo de clase:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

El `UpdateShoppingCartDatabase` método, se llama desde el `UpdateCartItems` método en el *ShoppingCart.aspx.cs* página, que contiene la lógica para actualizar o quitar elementos del carro de la compra. El `UpdateShoppingCartDatabase` método recorre en iteración todas las filas dentro de la lista del carro de la compra. Si un elemento del carro de la compra se ha marcado para quitarse o la cantidad es menor que uno, el `RemoveItem` se llama al método. En caso contrario, se busca el elemento del carro de la compra cuando actualiza el `UpdateItem` se llama al método. Una vez se haya eliminado o actualizado el elemento del carro de la compra, se guardan los cambios de la base de datos.

El `ShoppingCartUpdates` estructura se usa para contener todos los elementos del carro de la compra. El `UpdateShoppingCartDatabase` método usa el `ShoppingCartUpdates` estructura para determinar si cualquiera de los elementos deben actualizarse o quitarse.

En el siguiente tutorial, utilizará el `EmptyCart` método para borrar la compra el carro después de la compra de productos. Pero por ahora, usará el `GetCount` método que acaba de agregar a la *ShoppingCartActions.cs* archivo para determinar cuántos elementos hay en el carro de la compra.

### <a name="adding-a-shopping-cart-counter"></a>Agregar un contador de carro de la compra

Para permitir al usuario ver el número total de elementos en el carro de la compra, agregará un contador para el *Site.Master* página. Este contador también actuará como un vínculo a la cesta.

1. En **el Explorador de soluciones**, abra el *Site.Master* página.
2. Modificar el marcado agregando el vínculo del contador carro de la compra, como se muestra en amarillo en la sección de exploración para que aparezca como sigue:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. A continuación, actualice el código subyacente de la *Site.Master.cs* archivo agregando el código resaltado en amarillo como sigue:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Antes de la página se representa como HTML, el `Page_PreRender` provoca el evento. En el `Page_PreRender` controlador, el recuento total del carro de la compra se determina mediante una llamada a la `GetCount` método. El valor devuelto se agrega a la `cartCount` intervalo incluido en el marcado de la *Site.Master* página. El `<span>` etiquetas permite a los elementos internos se representen correctamente. Cuando se muestre cualquier página del sitio, se mostrará el total del carro de la compra. El usuario puede hacer clic en el total del carro de la compra para mostrar el carro de la compra.

## <a name="testing-the-completed-shopping-cart"></a>Pruebas completada carro de la compra

Puede ejecutar la aplicación ahora para ver cómo se puede agregar, eliminar y actualizar elementos en el carro de la compra. El total del carro de la compra reflejará el costo total de todos los elementos en el carro de la compra.

1. Presione **F5** para ejecutar la aplicación.  
 El explorador se abre y muestra el *Default.aspx* página.
2. Seleccione **automóviles** en el menú de navegación de la categoría.
3. Haga clic en el **Add To Cart** vínculo situado junto al primer producto.   
 El *ShoppingCart.aspx* página se muestra con el total del pedido.
4. Seleccione **planos** en el menú de navegación de la categoría.
5. Haga clic en el **Add To Cart** vínculo situado junto al primer producto.
6. Establezca la cantidad del primer elemento en el carro de la compra a 3 y seleccione el **quitar elemento** casilla de verificación del segundo elemento.<a id="a"></a>
7. Haga clic en el **actualizar** botón para actualizar la página del carro de la compra y mostrar el nuevo total del pedido. 

    ![Carro de compra - carro de la actualización](shopping-cart/_static/image9.png)

## <a name="summary"></a>Resumen

En este tutorial, ha creado un carro de la aplicación de ejemplo Wingtip Toys Web Forms. Durante este tutorial ha usado el Entity Framework Code First, las anotaciones de datos, controles de datos fuertemente tipados y enlace de modelos.

Carro de la compra admite agregar, eliminar y actualizar los elementos que el usuario ha seleccionado para su compra. Además de implementar la funcionalidad de carro de la compra, ha aprendido cómo mostrar los elementos del carro de la compra en un **GridView** controlar y calcular el total del pedido.

## <a name="addition-information"></a>Obtener información adicional

[Información general del estado de sesión de ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Anterior](display_data_items_and_details.md)
> [Siguiente](checkout-and-payment-with-paypal.md)
