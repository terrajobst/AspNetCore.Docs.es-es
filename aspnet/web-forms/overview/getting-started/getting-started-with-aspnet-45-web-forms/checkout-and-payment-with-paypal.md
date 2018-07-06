---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Retirada y pago con PayPal | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 3299da33a68f02ac1b3ffe7c037d06d8ece9455e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835636"
---
<a name="checkout-and-payment-with-paypal"></a>Retirada y pago con PayPal
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible para acompañar esta serie de tutoriales.


Este tutorial describe cómo modificar la aplicación de ejemplo Wingtip Toys para incluir la autorización del usuario, registro y el pago con PayPal. Solo los usuarios que iniciaron sesión tendrá autorización para adquirir productos. Funcionalidad de registro de usuario integradas de la plantilla de proyecto de ASP.NET 4.5 Web Forms ya incluye gran parte de lo que necesita. Agregará funcionalidad de finalización de Express de PayPal. En este tutorial se usa el programador de PayPal probar el entorno, por lo que no se transferirá ningún fondos reales. Al final del tutorial, probará la aplicación mediante la selección de productos que se va a agregar al carro de la compra, al hacer clic en el botón checkout y transferencia de datos en el sitio web de PayPal pruebas. En el sitio web de la prueba PayPal se confirme su información de pago y envío y, a continuación, volver a la aplicación de ejemplo Wingtip Toys local para confirmar y completar la compra.

Hay varios procesadores de pago de terceros con experiencia que se especializan en la compra en línea que abordan la escalabilidad y seguridad. Los desarrolladores de ASP.NET deben considerar las ventajas de utilizar una solución de pago de terceros antes de implementar una cesta y solución de compra.

> [!NOTE] 
> 
> La aplicación de ejemplo Wingtip Toys se diseñó para que se muestran los conceptos específicos de ASP.NET y las características disponibles para los desarrolladores web ASP.NET. Esta aplicación de ejemplo no se optimizó para todas las circunstancias posibles con respecto a la escalabilidad y seguridad.


## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo restringir el acceso a páginas específicas de una carpeta.
- Cómo crear un carro de compra conocido de un carro de compra anónimo.
- Cómo habilitar SSL para el proyecto.
- Cómo agregar un proveedor de OAuth para el proyecto.
- Cómo usar PayPal para comprar productos usando el entorno de pruebas de PayPal.
- Cómo mostrar detalles de PayPal en un **DetailsView** control.
- Cómo actualizar la base de datos de la aplicación Wingtip Toys con información obtenida de PayPal.

## <a name="adding-order-tracking"></a>Agregar seguimiento de pedidos

En este tutorial, creará dos nuevas clases para realizar un seguimiento de los datos del pedido que ha creado un usuario. Las clases realizará el seguimiento de datos con respecto a la información de envío, total de compra y la confirmación de pago.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Agregar el orden y las clases de modelo OrderDetail

Anteriormente en esta serie de tutoriales, se ha definido el esquema para las categorías, productos, y elementos del carro de la compra mediante la creación de la `Category`, `Product`, y `CartItem` clases en el *modelos* carpeta. Ahora agregará dos nuevas clases para definir el esquema para el pedido de producto y los detalles del pedido.

1. En el **modelos** carpeta, agregue una nueva clase denominada *Order.cs*.   
   El nuevo archivo de clase se muestra en el editor.
2. Reemplace el código predeterminado con lo siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Agregar un *OrderDetail.cs* clase a la *modelos* carpeta.
4. Reemplace el código predeterminado con el código siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

El `Order` y `OrderDetail` clases contienen el esquema para definir la información de orden utilizada para adquirir y envío.

Además, deberá actualizar la clase de contexto de base de datos que administra las clases de entidad y que proporciona acceso a datos en la base de datos. Para ello, agregará el orden recién creado y `OrderDetail` las clases de modelo `ProductContext` clase.

1. En **el Explorador de soluciones**, busque y abra el *ProductContext.cs* archivo.
2. Agregue el código resaltado a la *ProductContext.cs* archivo tal como se muestra a continuación:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Como se mencionó anteriormente en esta serie de tutoriales, el código en el *ProductContext.cs* archivo agrega el `System.Data.Entity` espacio de nombres para que tengan acceso a toda la funcionalidad núcleo de Entity Framework. Esta funcionalidad incluye la capacidad de consultar, insertar, actualizar y eliminar datos trabajando con objetos fuertemente tipados. El código anterior en el `ProductContext` clase agrega acceso de Entity Framework recién agregada `Order` y `OrderDetail` clases.

## <a name="adding-checkout-access"></a>Agregar acceso de desprotección

La aplicación de ejemplo Wingtip Toys permite a los usuarios anónimos revisar y agregar productos a un carro de la compra. Sin embargo, cuando los usuarios anónimos eligen los productos que se agregan a la cesta de compra, debe iniciar sesión en el sitio. Una vez que ha iniciado sesión, puede tener acceso a las páginas restringidas de la aplicación Web que controlan la desprotección y el proceso de compra. Estas páginas restringidas se encuentran en el *desprotección* carpeta de la aplicación.

### <a name="add-a-checkout-folder-and-pages"></a>Agregue una carpeta de desprotección y páginas

Ahora creará el *desprotección* carpeta y las páginas que los clientes verán durante el proceso de pago. Estas páginas se actualizará más adelante en este tutorial.

1. Haga clic en el nombre del proyecto (**Wingtip Toys**) en **el Explorador de soluciones** y seleccione **agregar una nueva carpeta**. 

    ![Retirada y pago con PayPal - nueva carpeta](checkout-and-payment-with-paypal/_static/image1.png)
2. Nombre de la nueva carpeta *desprotección*.
3. Haga clic en el *desprotección* carpeta y, a continuación, seleccione **agregar**-&gt;**nuevo elemento**. 

    ![Desprotección y el pago con PayPal: nuevo elemento](checkout-and-payment-with-paypal/_static/image2.png)
4. Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
5. Seleccione el **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda. A continuación, en el panel central, seleccione **formulario Web Forms con página maestra**y asígnele el nombre *CheckoutStart.aspx*. 

    ![Retirada y pago con PayPal - Agregar nuevo elemento de cuadro de diálogo](checkout-and-payment-with-paypal/_static/image3.png)
6. Como antes, seleccione el *Site.Master* archivo como la página maestra.
7. Agregue las siguientes páginas adicionales para el *desprotección* carpeta siguiendo los mismos pasos anteriores:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Agregue un archivo Web.config

Al agregar una nueva *Web.config* del archivo a la *desprotección* carpeta, podrá restringir el acceso a todas las páginas contenidas en la carpeta.

1. Haga clic en el *desprotección* carpeta y seleccione **agregar**  - &gt; **nuevo elemento**.  
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda. A continuación, en el panel central, seleccione **archivo de configuración Web**, acepte el nombre predeterminado de *Web.config*y, a continuación, seleccione **agregar**.
3. Reemplace el contenido XML del *Web.config* archivo por lo siguiente:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Guardar el *Web.config* archivo.

El *Web.config* archivo Especifica que todos los usuarios desconocidos de la aplicación Web deben ser deniega el acceso a las páginas contenidas en el *desprotección* carpeta. Sin embargo, si el usuario ha registrado una cuenta y haya iniciado sesión, que será un usuario conocido y tendrá acceso a las páginas de la *desprotección* carpeta.

Es importante tener en cuenta que la configuración de ASP.NET sigue una jerarquía, donde cada *Web.config* archivo aplica valores de configuración a la carpeta que se encuentra y a todos los directorios secundarios debajo de él.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Habilitar SSL para el proyecto

 Capa de Sockets seguros (SSL) es un protocolo definido para permitir que los servidores Web y los clientes Web comunicarse de forma más segura mediante el uso de cifrado. Cuando no se usa SSL, los datos enviados entre el cliente y el servidor está abiertos a cualquier persona con acceso físico a la red de rastreo de paquetes. Además, varios esquemas de autenticación habituales no son seguras a través de HTTP sin formato. En concreto, la autenticación básica y autenticación mediante formularios envían credenciales no cifradas. Para que sea seguro, estos esquemas de autenticación deben usar SSL. 

1. En **el Explorador de soluciones**, haga clic en el **WingtipToys** del proyecto, a continuación, presione **F4** para mostrar el **propiedades** ventana.
2. Cambio **SSL habilitado** a `true`.
3. Copia el **dirección URL de SSL** para que pueda usar más adelante.   
 La dirección URL de SSL será `https://localhost:44300/` a menos que haya creado previamente los sitios Web SSL (como se muestra a continuación).   
    ![Propiedades del proyecto](checkout-and-payment-with-paypal/_static/image4.png)
4. En **el Explorador de soluciones**, haga clic en el **WingtipToys** del proyecto y haga clic en **propiedades**.
5. En el panel izquierdo, haga clic en **Web**.
6. Cambiar el **dirección Url del proyecto** para usar el **dirección URL de SSL** que guardó anteriormente.   
    ![Propiedades del proyecto Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Guarde la página presionando **CTRL+S**.
8. Presione **Ctrl+F5** para ejecutar la aplicación. Visual Studio mostrará una opción para que pueda evitar advertencias de SSL.
9. Haga clic en **Sí** para confiar en el certificado SSL de IIS Express y continuar.   
    ![Detalles del certificado SSL de IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Se muestra una advertencia de seguridad.
10. Haga clic en **Sí** para instalar el certificado en el host local.   
    ![Cuadro de diálogo de advertencia de seguridad](checkout-and-payment-with-paypal/_static/image7.png)  
 Se mostrará la ventana del explorador.

Ahora puede probar fácilmente la aplicación Web localmente mediante SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Agregar un proveedor de OAuth 2.0

ASP.NET Web Forms proporciona opciones mejoradas para la pertenencia y autenticación. Estas mejoras incluyen OAuth. OAuth es un protocolo abierto que ofrece autorización segura en un método estándar sencillo de aplicaciones web, móviles y de escritorio. La plantilla de formularios Web Forms de ASP.NET utiliza OAuth para exponer Facebook, Twitter, Google y Microsoft como proveedores de autenticación. Aunque este tutorial solo utiliza Google como proveedor de autenticación, puede modificar fácilmente el código para usar cualquiera de los proveedores. Los pasos para implementar otros proveedores son muy similares a los pasos que verá en este tutorial.

Además de la autenticación, el tutorial utilizará también funciones para implementar la autorización. Solo los usuarios que agregue a la `canEdit` rol podrá cambiar los datos (crear, editar o eliminar contactos).

> [!NOTE] 
> 
> Las aplicaciones de Windows Live aceptan sólo una dirección URL en directo para un sitio Web en funcionamiento, por lo que no puede usar una dirección URL del sitio Web local para probar los inicios de sesión.


Los pasos siguientes, podrá agregar un proveedor de autenticación de Google.

1. Abra el *aplicación\_Start\Startup.Auth.cs* archivo.
2. Quite los caracteres de comentario de la `app.UseGoogleAuthentication()` método para que el método aparece como sigue: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Navegue hasta la [Google Developers Console](https://console.developers.google.com/). También necesitará iniciar sesión con su cuenta de correo electrónico del desarrollador de Google (gmail.com). Si no tiene una cuenta de Google, seleccione el **crear una cuenta** vínculo.   
   A continuación, verá el **Google Developers Console**.   
    ![Consola de desarrolladores de Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Haga clic en el **crear proyecto** botón y escriba un nombre de proyecto y el identificador (que puede usar los valores predeterminados). A continuación, haga clic en el **casilla de verificación de acuerdo** y **crear** botón.  

    ![Google: nuevo proyecto](checkout-and-payment-with-paypal/_static/image9.png)

   En unos segundos se creará el nuevo proyecto y el explorador mostrará la nueva página de proyectos.
5. En el panel izquierdo, haga clic en **API &amp; auth**y, a continuación, haga clic en **credenciales**.
6. Haga clic en el **crear nuevo Id. de cliente** en **OAuth**.   
   El **crear Id. de cliente** se mostrará el cuadro de diálogo.   
    ![Google - crear Id. de cliente](checkout-and-payment-with-paypal/_static/image10.png)
7. En el **crear Id. de cliente** cuadro de diálogo, mantenga el valor predeterminado **aplicación Web** para el tipo de aplicación.
8. Establecer el **orígenes de JavaScript autorizados** a la dirección URL de SSL que usó anteriormente en este tutorial (`https://localhost:44300/` a menos que haya creado a otros proyectos SSL).   
   Esta dirección URL es el origen de la aplicación. Para este ejemplo, debe proporcionar solo la dirección URL de prueba de localhost. Sin embargo, puede especificar varias direcciones URL para tener en cuenta para localhost y producción.
9. Establecer el **URI de redireccionamiento autorizado** al siguiente: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Este valor es el URI que utiliza ASP.NET OAuth para comunicarse con el servidor de OAuth de google. Recuerde que la dirección URL de SSL que usó anteriormente ( `https://localhost:44300/` a menos que haya creado a otros proyectos SSL).
10. Haga clic en el **crear Id. de cliente** botón.
11. En el menú izquierdo de la consola de desarrolladores de Google, haga clic en el **pantalla de consentimiento** elemento de menú, a continuación, establezca el nombre de producto y la dirección de correo electrónico. Cuando haya completado el formulario, haga clic en **guardar**.
12. Haga clic en el **API** elemento de menú, desplácese hacia abajo y haga clic en el **desactivar** situado junto a **Google + API**.   
    Acepta esta opción permitirá a la API de Google +.
13. También debe actualizar el **Microsoft.Owin** versión 3.0.0 del paquete NuGet.   
    Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet** y, a continuación, seleccione **administrar paquetes de NuGet para la solución**.  
    Desde el **administrar paquetes de NuGet** ventana, buscar y actualizar la **Microsoft.Owin** paquete a la versión 3.0.0.
14. En Visual Studio, actualice el `UseGoogleAuthentication` método de la *Startup.Auth.cs* página copiando y pegando el **Id. de cliente** y **secreto de cliente** al método. El **Id. de cliente** y **secreto de cliente** son ejemplos de valores que se muestran a continuación y no funcionará. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Presione **CTRL+F5** para compilar y ejecutar la aplicación. Haga clic en el **iniciarla** vínculo.
16. En **utilice otro servicio para iniciar sesión**, haga clic en **Google**.  
    ![Inicia sesión](checkout-and-payment-with-paypal/_static/image11.png)
17. Si tiene que escribir sus credenciales, se le redirigirá al sitio de google donde especificará sus credenciales.  
    ![Google: inicio de sesión](checkout-and-payment-with-paypal/_static/image12.png)
18. Después de escribir sus credenciales, se le pedirá para conceder permisos a la aplicación web que acaba de crear.  
    ![Cuenta de servicio de proyecto predeterminada](checkout-and-payment-with-paypal/_static/image13.png)
19. Haga clic en **acepte**. Se le redirigirá ahora a la **registrar** página de la **WingtipToys** aplicación donde puede registrar su cuenta de Google.  
    ![Registrar con su cuenta de Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Tiene la opción de cambiar el nombre de registro de correo electrónico local utilizado para la cuenta de Gmail, pero generalmente deseará mantener el alias de correo electrónico predeterminada (es decir, lo que se usó para la autenticación). Haga clic en **iniciarla** como se indicó anteriormente.

### <a name="modifying-login-functionality"></a>Modificar la funcionalidad de inicio de sesión

Como se mencionó anteriormente en esta serie de tutoriales, gran parte de la funcionalidad de registro de usuario se ha incluido en la plantilla de formularios Web Forms de ASP.NET de forma predeterminada. Ahora, modificará el valor predeterminado *Login.aspx* y *Register.aspx* páginas para llamar a la `MigrateCart` método. El `MigrateCart` método asocia un usuario recién registrado en un carro de compra anónimo. Al asociar el usuario y carro de la compra, la aplicación de ejemplo Wingtip Toys será capaz de mantener el carro de la compra del usuario entre las visitas.

1. En **el Explorador de soluciones**, busque y abra el *cuenta* carpeta.
2. Modificar la página de código subyacente denominada *Login.aspx.cs* para incluir el código resaltado en amarillo, para que aparezca como sigue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Guardar el *Login.aspx.cs* archivo.

Por ahora, puede omitir la advertencia de que no hay ninguna definición para el `MigrateCart` método. Va a agregar, un poco más adelante en este tutorial.

El *Login.aspx.cs* archivo de código subyacente admite un método de inicio de sesión. Mediante la inspección de la página Login.aspx, verá que esta página incluye un botón "Iniciar sesión" que, cuando haga clic en los desencadenadores del `LogIn` controlador en el código subyacente.

Cuando el `Login` método en el *Login.aspx.cs* se denomina una nueva instancia de la cesta denominado `usersShoppingCart` se crea. El identificador del carro de la compra (GUID) se recupera y se establece en el `cartId` variable. A continuación, la `MigrateCart` método se llama, pasando tanto el `cartId` y el nombre del usuario ha iniciado la sesión a este método. Cuando se migra el carro de la compra, el GUID utilizado para identificar el carro de la compra anónimo se reemplaza por el nombre de usuario.

Además de modificar el *Login.aspx.cs* archivo de código subyacente para migrar el carro de la compra cuando el usuario inicia sesión, también debe modificar el *archivo de código subyacente Register.aspx.cs* para migrar el carro de la compra Cuando el usuario crea una nueva cuenta y se inicia sesión.

1. En el *cuenta* carpeta, abra el archivo de código subyacente denominado *Register.aspx.cs*.
2. Modifique el archivo de código subyacente incluyendo el código en amarillo, para que aparezca como sigue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Guardar el *Register.aspx.cs* archivo. Una vez más, pasar por alto la advertencia sobre el `MigrateCart` método.

Tenga en cuenta que el código usado en el `CreateUser_Click` controlador de eventos es muy similar al código que usó en el `LogIn` método. Cuando el usuario se registra o se inicia sesión en el sitio, una llamada a la `MigrateCart` método se realizará.

## <a name="migrating-the-shopping-cart"></a>Migrar el carro de la compra

Ahora que tiene el proceso de inicio de sesión y registro actualizado, puede agregar el código para migrar el uso de carro de la compra el `MigrateCart` método.

1. En **el Explorador de soluciones**, busque el *lógica* carpeta y abra el *ShoppingCartActions.cs* archivo de clase.
2. Agregue el código resaltado en amarillo en el código existente en el *ShoppingCartActions.cs* archivo, por lo que el código en el *ShoppingCartActions.cs* archivo aparece como sigue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

El `MigrateCart` método usa el cartId existente para buscar el carro de la compra del usuario. A continuación, el código recorre en bucle todos los elementos del carro de la compra y reemplaza el `CartId` propiedad (según lo especificado por el `CartItem` esquema) con el nombre de usuario que inició sesión.

### <a name="updating-the-database-connection"></a>Actualización de la conexión de base de datos

Si está siguiendo este tutorial utilizando la **creados previamente** Wingtip Toys aplicación de ejemplo, debe volver a crear la base de datos de pertenencia predeterminado. Al modificar la cadena de conexión de forma predeterminada, la base de datos de pertenencia se creará la próxima vez que se ejecuta la aplicación.

1. Abra el *Web.config* archivo en la raíz del proyecto.
2. Actualice la cadena de conexión predeterminado para que aparezca como sigue:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>La integración de PayPal

PayPal es una plataforma de facturación basada en web que acepte pagos por los comercios en línea. A continuación, en este tutorial se explica cómo integrar la funcionalidad de PayPal Express desprotección en la aplicación. Desprotección Express permite a los clientes usar PayPal para pagar los elementos que se hayan agregado a su carro de la compra.

### <a name="create-paylpal-test-accounts"></a>Crear cuentas de prueba PaylPal

Para usar el entorno de pruebas de PayPal, debe crear y comprobar una cuenta de prueba para desarrolladores. Usará la cuenta de prueba para desarrolladores para crear un comprador de cuenta de prueba y una cuenta de prueba de vendedor. Las credenciales de cuenta de prueba para desarrolladores también permitirá la aplicación de ejemplo Wingtip Toys tener acceso al entorno de prueba de PayPal.

1. En un explorador, vaya a los desarrolladores de PayPal probar sitio:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Si no tienes una cuenta de desarrollador de PayPal, cree una nueva cuenta haciendo **Sign Up**y siga los pasos de registro. Si tiene una cuenta de desarrollador de PayPal, inicie sesión haciendo **en el registro**. Necesitará la cuenta de desarrollador de PayPal para probar la aplicación de ejemplo Wingtip Toys más adelante en este tutorial.
3. Si solo se ha registrado para la cuenta de desarrollador de PayPal, deberá comprobar su cuenta de desarrollador de PayPal con PayPal. Puede comprobar su cuenta siguiendo los pasos que PayPal enviada a su cuenta de correo electrónico. Una vez que haya comprobado la cuenta de desarrollador de PayPal, inicie sesión en el sitio de pruebas de los desarrolladores PayPal.
4. Después de que ha iniciado la sesión el sitio para desarrolladores de PayPal con su cuenta de desarrollador de PayPal, deberá crear una cuenta de prueba de comprador de PayPal si no lo hace ya tiene uno. Para crear una cuenta de prueba de comprador en el sitio de PayPal, haga clic en el **aplicaciones** pestaña y, a continuación, haga clic en **cuentas de espacio aislado**.   
 El **cuentas de prueba de espacio aislado** se muestra la página.   

    > [!NOTE] 
    > 
    > El sitio para desarrolladores de PayPal ya proporciona una cuenta de comerciante de prueba.

    ![Pago con PayPal - cuentas de prueba de espacio aislado y envío](checkout-and-payment-with-paypal/_static/image15.png)
5. En la página de cuentas de prueba de recinto de seguridad, haga clic en **crear cuenta**.
6. En el **crear cuenta de prueba** página Elegir un comprador de correo electrónico de la cuenta de prueba y la contraseña de su elección.   

    > [!NOTE] 
    > 
    > Necesitará las direcciones de correo electrónico de comprador y la contraseña para probar la aplicación de ejemplo Wingtip Toys al final de este tutorial.

    ![Pago con PayPal - cuentas de prueba de espacio aislado y envío](checkout-and-payment-with-paypal/_static/image16.png)
7. Crear la cuenta de prueba de comprador haciendo clic en el **crear cuenta** botón.  
 El **cuentas de prueba de espacio aislado** se muestra la página. 

    ![Pago con PayPal - PaylPal cuentas y envío](checkout-and-payment-with-paypal/_static/image17.png)
8. En el **cuentas de prueba de espacio aislado** página, haga clic en el **facilitador** cuentas de correo electrónico.  
    **Perfil** y **notificación** opciones que aparecen.
9. Seleccione el **perfil** opción y, después, haga clic en **las credenciales de la API** para ver sus credenciales de la API para la cuenta de comerciante de prueba.
10. Copiar las credenciales de la API de prueba en el Bloc de notas.

Necesitará las credenciales de la API clásica de prueba mostradas (Username, Password y firma) para realizar llamadas de API de la aplicación de ejemplo Wingtip Toys en el entorno de pruebas de PayPal. Agregará las credenciales en el paso siguiente.

### <a name="add-paypal-class-and-api-credentials"></a>Agregar clase de PayPal y las credenciales de la API

Colocará la mayoría del código de PayPal en una sola clase. Esta clase contiene los métodos usados para comunicarse con PayPal. Además, agregará sus credenciales de PayPal a esta clase.

1. En la aplicación de ejemplo Wingtip Toys dentro de Visual Studio, haga clic en el **lógica** carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. En **Visual C#** desde el **instalado** panel de la izquierda, seleccione **código**.
3. En el panel central, seleccione **clase**. Esta nueva clase el nombre **PayPalFunctions.cs**.
4. Haga clic en **Agregar**.  
   El nuevo archivo de clase se muestra en el editor.
5. Reemplace el código predeterminado con el código siguiente:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Agregue las credenciales de la API de comerciante (Username, Password y firma) que se ha mostrado anteriormente en este tutorial para que puedan realizar llamadas de función en el entorno de prueba de PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> En esta aplicación de ejemplo simplemente va a agregar las credenciales a un archivo de C# (. cs). Sin embargo, en una solución implementada, debe considerar sus credenciales en un archivo de configuración de cifrado.


La clase NVPAPICaller contiene la mayor parte de la funcionalidad de PayPal. El código de la clase proporciona los métodos necesarios para realizar una prueba de compra desde el entorno de pruebas de PayPal. Las tres funciones de PayPal siguientes se utilizan para realizar compras:

- `SetExpressCheckout` Función
- `GetExpressCheckoutDetails` Función
- `DoExpressCheckoutPayment` Función

El `ShortcutExpressCheckout` método recopila los detalles de producto y la información de compra de prueba de la cesta y llama a la `SetExpressCheckout` función PayPal. El `GetCheckoutDetails` método confirma los detalles de la compra y llama a la `GetExpressCheckoutDetails` PayPal función antes de realizar la compra de prueba. El `DoCheckoutPayment` método completa la compra de prueba desde el entorno de pruebas mediante una llamada a la `DoExpressCheckoutPayment` función PayPal. El código restante es compatible con los métodos de PayPal y proceso, como la codificación de cadenas, descodificación de cadenas, matrices de procesamiento y determinar las credenciales.

> [!NOTE] 
> 
> PayPal permite incluir detalles de la compra opcional según [especificación de API de PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Al extender el código de la aplicación de ejemplo Wingtip Toys, puede incluir detalles de localización, descripciones de productos, impuestos, un número de servicio al cliente, así como muchos otros campos opcionales.


Tenga en cuenta que las direcciones URL de retorno y cancelación que se especifican en el **ShortcutExpressCheckout** método usa un número de puerto.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Cuando Visual Web Developer ejecuta un proyecto web mediante SSL, normalmente se usa el puerto 44300 para el servidor web. Como se indicó anteriormente, el número de puerto es 44300. Al ejecutar la aplicación, podría ver un número de puerto diferente. Sus necesidades de número de puerto debe establecerse correctamente en el código para que pueda correcta ejecutan la aplicación de ejemplo Wingtip Toys al final de este tutorial. La siguiente sección de este tutorial explica cómo recuperar el número de puerto del host local y actualice la clase de PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Actualizar el número de puerto de host local en la clase de PayPal

La aplicación de ejemplo Wingtip Toys compras de productos, vaya a la página de prueba de PayPal y volver a la instancia local de la aplicación de ejemplo Wingtip Toys. Para disponer de PayPal volver a la dirección URL correcta, deberá especificar el número de puerto de la ejecución localmente la aplicación en el código de PayPal mencionado anteriormente de ejemplo.

1. Haga clic en el nombre del proyecto (**WingtipToys**) en **el Explorador de soluciones** y seleccione **propiedades**.
2. En la columna izquierda, seleccione el **Web** ficha.
3. Recuperar el número de puerto desde el **dirección Url del proyecto** cuadro.
4. Si es necesario, actualice el `returnURL` y `cancelURL` en la clase de PayPal (`NVPAPICaller`) en el *PayPalFunctions.cs* archivo que se usará el número de puerto de la aplicación web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Ahora el código que agregó hará coincidir el puerto esperado para la aplicación Web local. PayPal podrá volver a la dirección URL correcta en el equipo local.

### <a name="add-the-paypal-checkout-button"></a>Agregar el botón Checkout PayPal

Ahora que las funciones principales de PayPal se agregaron a la aplicación de ejemplo, puede empezar a agregar el marcado y el código necesario para llamar a estas funciones. En primer lugar, debe agregar el botón checkout que verá el usuario en la página del carro de la compra.

1. Abra el *ShoppingCart.aspx* archivo.
2. Desplácese hasta la parte inferior del archivo y busque el `<!--Checkout Placeholder -->` comentario.
3. Reemplace el comentario con un `ImageButton` controlar de manera que la marca de seguridad se sustituye por las siguientes:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. En el *ShoppingCart.aspx.cs* archivo, después el `UpdateBtn_Click` controlador de eventos cerca del final del archivo, agregue el `CheckOutBtn_Click` controlador de eventos:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. También en el *ShoppingCart.aspx.cs* , agregue una referencia a la `CheckoutBtn`, de modo que el nuevo botón de imagen se hace referencia como sigue:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Guarde los cambios en ambos el *ShoppingCart.aspx* archivo y la *ShoppingCart.aspx.cs* archivo.
7. En el menú, seleccione **depurar**-&gt;**WingtipToys compilar**.  
   Se volverá a generar el proyecto con la recién agregada **ImageButton** control.

### <a name="send-purchase-details-to-paypal"></a>Enviar detalles de la compra a PayPal

Cuando el usuario hace clic en el **desprotección** botón en la página del carro de la compra (*ShoppingCart.aspx*), comenzará el proceso de compra. El código siguiente llama a la primera función de PayPal necesitada para comprar productos.

1. Desde el *desprotección* carpeta, abra el archivo de código subyacente denominado *CheckoutStart.aspx.cs*.   
   Asegúrese de abrir el archivo de código subyacente.
2. Reemplace el código existente por el siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Cuando el usuario de la aplicación hace clic en el **desprotección** botón en la página del carro de la compra, el explorador se le remitirá a la *CheckoutStart.aspx* página. Cuando el *CheckoutStart.aspx* , se carga la página el `ShortcutExpressCheckout` se llama al método. En este momento, el usuario se transfiere al sitio web de prueba PayPal. En el sitio de PayPal, el usuario escribe sus credenciales de PayPal, revisa los detalles de la compra, acepta el contrato de PayPal y devuelve a la aplicación de ejemplo Wingtip Toys donde el `ShortcutExpressCheckout` método se completa. Cuando el `ShortcutExpressCheckout` método se haya completado, se le redirigirá al usuario la *CheckoutReview.aspx* página especificada en el `ShortcutExpressCheckout` método. Esto permite al usuario revisar los detalles del pedido desde dentro de la aplicación de ejemplo Wingtip Toys.

### <a name="review-order-details"></a>Revisar los detalles de pedido

Después de volver de PayPal, el *CheckoutReview.aspx* página de la aplicación de ejemplo Wingtip Toys muestra los detalles del pedido. Esta página permite al usuario revisar los detalles del pedido antes de adquirir los productos. El *CheckoutReview.aspx* página se debe crear como sigue:

1. En el *desprotección* carpeta, abra la página denominada *CheckoutReview.aspx*.
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Abra la página de código subyacente denominada *CheckoutReview.aspx.cs* y reemplace el código existente con lo siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

El **DetailsView** control se usa para mostrar los detalles del pedido que se han devuelto de PayPal. Además, el código anterior guarda los detalles del pedido a la base de datos de Wingtip Toys como un `OrderDetail` objeto. Cuando el usuario hace clic en el **completar pedido** botón, se les redirigirá a la *CheckoutComplete.aspx* página.

> [!NOTE] 
> 
> **Sugerencia**
> 
> En el marcado de la *CheckoutReview.aspx* página, observe que la `<ItemStyle>` etiqueta a la que se usa para cambiar el estilo de los elementos dentro de la **DetailsView** control cerca de la parte inferior de la página. Mediante la visualización de la página en **vista de diseño** (seleccionando **diseño** en la esquina inferior izquierda de Visual Studio), a continuación, seleccione el **DetailsView** control y seleccionando el  **Etiqueta inteligente** (el icono de flecha en la parte superior derecha del control), podrá ver el **DetailsView tareas**.
> 
> ![Desprotección y el pago con PayPal: editar campos](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Seleccionando **editar campos**, **campos** aparecerá el cuadro de diálogo. En este cuadro de diálogo puede controlar fácilmente las propiedades visuales, tales como **ItemStyle**, de la **DetailsView** control.
> 
> ![Pago con PayPal - cuadro de diálogo campos y envío](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Completar la compra

*CheckoutComplete.aspx* página realiza la compra de PayPal. Como se mencionó anteriormente, el usuario debe hacer clic en el **completar pedido** botón antes de la aplicación se le remitirá a la *CheckoutComplete.aspx* página.

1. En el *desprotección* carpeta, abra la página denominada *CheckoutComplete.aspx*.
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Abra la página de código subyacente denominada *CheckoutComplete.aspx.cs* y reemplace el código existente con lo siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Cuando el *CheckoutComplete.aspx* se carga la página, el `DoCheckoutPayment` se llama al método. Como se mencionó anteriormente, el `DoCheckoutPayment` método completa la compra desde el entorno de pruebas de PayPal. Una vez completada la compra del pedido, PayPal el *CheckoutComplete.aspx* página muestra una transacción de pago `ID` al comprador.

### <a name="handle-cancel-purchase"></a>Controlar Cancelar compra

Si el usuario decide cancelar la compra, se dirigirá a la *CheckoutCancel.aspx* donde verá que se ha cancelado su orden de página.

1. Abra la página llamada *CheckoutCancel.aspx* en el *desprotección* carpeta.
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Controlar los errores de compra

Errores durante el proceso de compra se controlarán mediante la *CheckoutError.aspx* página. El código subyacente de la *CheckoutStart.aspx* página, el *CheckoutReview.aspx* página y el *CheckoutComplete.aspx* cada página le redireccionará a la  *CheckoutError.aspx* página si se produce un error.

1. Abra la página llamada *CheckoutError.aspx* en el *desprotección* carpeta.
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

El *CheckoutError.aspx* página se muestra con los detalles del error cuando se produce un error durante el proceso de pago.

## <a name="running-the-application"></a>Ejecutar la aplicación

Ejecute la aplicación para ver cómo comprar productos. Tenga en cuenta que va a ejecutar en el PayPal entorno de pruebas. No se está intercambiando dinero real.

1. Asegúrese de que todos los archivos se guardan en Visual Studio.
2. Abra un explorador Web y vaya a [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Inicie sesión con su cuenta de desarrollador de PayPal que creó anteriormente en este tutorial.  
   Espacio aislado de desarrollador de PayPal, deberá haber iniciado sesión en [ https://developer.paypal.com ](https://developer.paypal.com/) para probar la desprotección de express. Esto solo se aplica a sandbox de PayPal las pruebas, no al entorno de producción de PayPal.
4. En Visual Studio, presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
   Una vez que vuelve a generar la base de datos, el explorador se abrirá y mostrará el *Default.aspx* página.
5. Agregar tres productos distintos al carro de la compra seleccionando la categoría de producto, por ejemplo, "Automóviles" y, a continuación, haga clic en **agregar al carro** junto a cada producto.  
   Carro de la compra mostrará el producto que ha seleccionado.
6. Haga clic en el **PayPal** botón Desproteger. 

    ![Carro de la desprotección y el pago con PayPal.](checkout-and-payment-with-paypal/_static/image20.png)

   Extrayendo del repositorio es necesario que tenga una cuenta de usuario para la aplicación de ejemplo Wingtip Toys.
7. Haga clic en el **Google** vínculo a la derecha de la página para iniciar sesión con una cuenta de correo electrónico de gmail.com existente.  
   Si no tiene una cuenta de gmail.com, puede crear uno para realizar pruebas en [www.gmail.com](https://www.gmail.com/). También puede usar una cuenta local estándar, haga clic en "Register". 

    ![Inicie sesión desprotección y el pago con PayPal.](checkout-and-payment-with-paypal/_static/image21.png)
8. Inicie sesión con su cuenta de gmail y la contraseña. 

    ![Retirada y pago con PayPal - inicio de sesión de Gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Haga clic en el **iniciarla** botón registrar su cuenta de gmail con su nombre de usuario de aplicación de ejemplo Wingtip Toys. 

    ![Retirada y pago con PayPal - registro de cuenta](checkout-and-payment-with-paypal/_static/image23.png)
10. En el sitio de prueba de PayPal, agregue su **comprador** enviar por correo electrónico a la dirección y la contraseña que creó anteriormente en este tutorial, a continuación, haga clic en el **sesión** botón. 

    ![Retirada y pago con PayPal - inicio de sesión PayPal](checkout-and-payment-with-paypal/_static/image24.png)
11. Aceptar la directiva de PayPal y haga clic en el **Acepto y continuar** botón.  
    Tenga en cuenta que esta página solo muestra la primera vez que use esta cuenta PayPal. Nuevamente, observe que se trata de una cuenta de prueba, no se intercambia dinero real. 

    ![Retirada y pago con PayPal - directiva de PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Revise la información de pedido en la comprobación de página de revisión del entorno y haga clic en de PayPal **continuar**. 

    ![Pago con PayPal - revise la información y envío](checkout-and-payment-with-paypal/_static/image26.png)
13. En el *CheckoutReview.aspx* página, compruebe la cantidad del pedido y ver la dirección de envío generado. A continuación, haga clic en el **completar pedido** botón. 

    ![Pago con PayPal: revisión de pedidos y envío](checkout-and-payment-with-paypal/_static/image27.png)
14. El **CheckoutComplete.aspx** página se muestra con un identificador de transacción de pago. 

    ![Retirada y pago con PayPal - desprotección completa](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Revisión de la base de datos

Al revisar los datos actualizados en la base de datos de aplicación de ejemplo Wingtip Toys después de ejecutar la aplicación, puede ver que la aplicación registrada correctamente la compra de los productos.

Puede inspeccionar los datos contenidos en el *Wingtiptoys.mdf* archivo de base de datos mediante el uso de la **Database Explorer** ventana (**Explorador de servidores** ventana de Visual Studio) al igual que anteriormente en esta serie de tutoriales.

1. Si todavía está abierto, cierre la ventana del explorador.
2. En Visual Studio, seleccione el **mostrar todos los archivos** situado en la parte superior de **el Explorador de soluciones** para que pueda expandir la **aplicación\_datos** carpeta.
3. Expanda el **aplicación\_datos** carpeta.  
 Es posible que deba seleccionar el **mostrar todos los archivos** icono de la carpeta.
4. Haga clic en el *Wingtiptoys.mdf* el archivo de base de datos y seleccione **abierto**.  
    **Explorador de servidores** se muestra.
5. Expanda el **tablas** carpeta.
6. Haga clic en el **pedidos**de tabla y seleccione **mostrar datos de tabla**.  
 El **pedidos** se muestra la tabla.
7. Revise el **PaymentTransactionID** columna para confirmar transacciones correctas. 

    ![Pago con PayPal - base de datos de revisión y envío](checkout-and-payment-with-paypal/_static/image29.png)
8. Cerrar la **pedidos** ventana de la tabla.
9. En el Explorador de servidores, haga clic en el **OrderDetails** de tabla y seleccione **mostrar datos de tabla**.
10. Revise el `OrderId` y `Username` valores en el **OrderDetails** tabla. Tenga en cuenta que estos valores coinciden con la `OrderId` y `Username` valores incluidos en el **pedidos** tabla.
11. Cerrar la **OrderDetails** ventana de la tabla.
12. Haga clic en el archivo de base de datos de Wingtip Toys (*Wingtiptoys.mdf*) y seleccione **cerrar conexión**.
13. Si no ve el **el Explorador de soluciones** ventana, haga clic en **el Explorador de soluciones** en la parte inferior de la **Explorador de servidores** ventana para mostrar el **el Explorador de soluciones**  nuevo.

## <a name="summary"></a>Resumen

En este tutorial, ha agregado order y esquemas de detalle de pedido para realizar un seguimiento de la compra de productos. También se integra la funcionalidad de PayPal en la aplicación de ejemplo Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionales

[Información general sobre la configuración de ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Implementar una aplicación de formularios Web ASP.NET segura con pertenencia, OAuth y SQL Database en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Declinación de responsabilidades

Este tutorial contiene código de ejemplo. Este código de ejemplo se proporciona "tal cual" sin garantía de ningún tipo. En consecuencia, Microsoft no garantiza la precisión, integridad o la calidad del código de ejemplo. Usted acepta usar el código de ejemplo en su propia responsabilidad. En ningún caso Microsoft será responsable para usted de ninguna manera cualquier código de ejemplo, contenido, incluyendo pero sin limitarse a algún error u omisión en cualquier código de ejemplo, contenido, o cualquier pérdida o daño de ningún tipo que se incurre por el uso del código de ejemplo. Por el presente documento se le notificará y quedará acepta indemnizar, guardar y almacenar frente y contra la pérdida de todo, notificaciones de pérdida, lesiones o daños de cualquier las ocasionados por incluidas, sin limitación, tipo o derivados de material que se registra, Microsoft transmitir, usar o se basan en incluir, pero no se limita a los puntos de vista expresados en él.

> [!div class="step-by-step"]
> [Anterior](shopping-cart.md)
> [Siguiente](membership-and-administration.md)
