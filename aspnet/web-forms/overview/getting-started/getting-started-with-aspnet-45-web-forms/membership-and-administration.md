---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Pertenencia y administración | Documentos de Microsoft
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 166bc642ea2083f455be0648e424f0b0ae3b082c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
<a name="membership-and-administration"></a>Pertenencia y administración
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar libros electrónicos (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible como acompañamiento de esta serie de tutoriales.


Este tutorial muestra cómo actualizar la aplicación de ejemplo Wingtip Toys para agregar un rol personalizado y usar ASP.NET Identity. También muestra cómo implementar una página de administración desde el que el usuario con un rol personalizado puede agregar y quitar productos desde el sitio Web.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) es el sistema de pertenencia que se usa para crear aplicaciones web ASP.NET y está disponible en ASP.NET 4.5. ASP.NET Identity se utiliza en la plantilla de proyecto de formularios Web Forms de Visual Studio 2013, así como las plantillas de [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), y [única página de aplicación de ASP.NET](../../../../single-page-application/index.md). Específicamente, puede instalar el sistema de identidades de ASP.NET mediante NuGet cuando se inicia con una aplicación Web vacía. Sin embargo, en esta serie de tutoriales utiliza el **formularios Web Forms**projecttemplate, que incluye el sistema de identidades de ASP.NET. ASP.NET Identity resulta muy sencillo integrar datos de perfil de usuario específica con datos de la aplicación. Además, ASP.NET Identity permite elegir el modelo de persistencia para perfiles de usuario en la aplicación. Puede almacenar los datos en una base de datos de SQL Server o en otro almacén de datos, incluidos los *NoSQL* almacenes de datos como tablas de almacenamiento de Windows Azure.

Este tutorial se basa en el tutorial anterior titulado "Desprotección y pago con PayPal" en la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo utilizar código para agregar un rol personalizado y un usuario a la aplicación.
- Cómo restringir el acceso a la carpeta de administración y la página.
- Cómo proporcionar métodos de navegación para el usuario que pertenece a la función personalizada.
- Cómo utilizar el enlace de modelos para rellenar un [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) control a categorías de producto.
- Cómo cargar un archivo en la aplicación web mediante la [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) control.
- Cómo usar controles de validación para implementar la validación de entrada.
- Cómo agregar y quitar los productos de la aplicación.

## <a name="these-features-are-included-in-the-tutorial"></a>Estas características se incluyen en el tutorial:

- ASP.NET Identity
- Configuración y la autorización
- Enlace de modelos
- Validación discreto

ASP.NET Web Forms proporciona capacidades de pertenencia. Mediante el uso de la plantilla predeterminada, tiene la funcionalidad de pertenencia integrados que puede usar inmediatamente cuando se ejecuta la aplicación. Este tutorial muestra cómo usar ASP.NET Identity para agregar un rol personalizado y asignar a un usuario a ese rol. Obtendrá información sobre cómo restringir el acceso a la carpeta de administración. Se agregará una página a la carpeta de administración que permite a un usuario con una función personalizada para agregar y quitar productos y para obtener una vista previa de un producto después de que se ha agregado.

## <a name="adding-a-custom-role"></a>Agregar un rol personalizado

Uso de la identidad de ASP.NET, puede agregar un rol personalizado y asignar a un usuario a ese rol mediante código.

1. En **el Explorador de soluciones**, haga doble clic en el *lógica* carpeta y cree una nueva clase.
2. La nueva clase el nombre *RoleActions.cs*.
3. Modifique el código para que aparezca como sigue:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. En **el Explorador de soluciones**, abra el *Global.asax.cs* archivo.
5. Modificar el *Global.asax.cs* archivo agregando el código resaltado en amarillo para que aparezca como sigue:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Tenga en cuenta que `AddUserAndRole` está subrayado en rojo. Haga doble clic en el código de AddUserAndRole.  
   La letra "A" al principio del método resaltado quedará subrayada.
7. Mantenga el mouse sobre la letra "A" y haga clic en la interfaz de usuario que le permite generar un código auxiliar del método para el `AddUserAndRole` método. 

    ![Pertenencia y Advministration - generar código auxiliar del método](membership-and-administration/_static/image1.png)
8. Haga clic en la opción titulada:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Abra la *RoleActions.cs* de archivos desde el *lógica* carpeta.  
   El `AddUserAndRole` método se ha agregado al archivo de clase.
10. Modificar el *RoleActions.cs* archivo mediante la eliminación de la `NotImplementedeException` y agregue el código que se resalta en amarillo, para que aparezca como sigue:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

El código anterior primero establece un contexto de base de datos para la base de datos de pertenencia. La base de datos de pertenencia también se almacena como un *.mdf* un archivo en el *aplicación\_datos* carpeta. Podrá ver esta base de datos una vez que el primer usuario ha iniciado sesión en esta aplicación web. 

> [!NOTE] 
> 
> Si desea almacenar los datos de pertenencia junto con los datos de producto, puede considerar el uso de la misma **DbContext** que usa para almacenar los datos de producto en el código anterior.


 El *interno* palabra clave es un modificador de acceso de tipos (por ejemplo, las clases) y miembros (por ejemplo, métodos o propiedades). Los tipos internos o los miembros son accesibles solo dentro de los archivos contenidos en el mismo ensamblado *(.dll* archivo). Cuando se compila la aplicación, un archivo de ensamblado *(.dll*) se crea que contiene el código que se ejecuta cuando se ejecuta la aplicación. 

Un `RoleStore` object, que proporciona administración de roles, se crea basándose en el contexto de base de datos.

> [!NOTE] 
> 
> Tenga en cuenta que, cuando la `RoleStore` se crea el objeto usa un tipo genérico `IdentityRole` tipo. Esto significa que la `RoleStore` sólo puede contener `IdentityRole` objetos. También mediante el uso de genéricos, los recursos de memoria se administran mejor.


Después, el `RoleManager` de objetos, se crea basándose en la `RoleStore` objeto que acaba de crear. el `RoleManager` objeto expone funciones relacionadas con la API que se puede usar para guardar automáticamente los cambios en el `RoleStore`. El `RoleManager` sólo puede contener `IdentityRole` objetos porque el código utiliza el `<IdentityRole>` tipo genérico.

Se llama a la `RoleExists` método para determinar si el rol "canEdit" está presente en la base de datos de pertenencia. Si no es así, puede crear el rol.

Crear el `UserManager` objeto parece ser más complicado que el `RoleManager` controlar, sin embargo es prácticamente los mismos. Solo se codifica en una línea en lugar de varios. En este caso, el parámetro que está pasando está creando instancias como un nuevo objeto contenido en los paréntesis.

A continuación cree el usuario "canEditUser" al crear un nuevo `ApplicationUser` objeto. A continuación, si consigue crear el usuario, agregue el usuario a la nueva función.

> [!NOTE] 
> 
> El control de errores se actualizará durante el tutorial "Control de errores de ASP.NET" más adelante en esta serie de tutoriales.


La próxima vez que se inicia la aplicación, se agregará el usuario denominado "canEditUser" como la función denominada "canEdit" de la aplicación. Más adelante en este tutorial, podrá iniciar una sesión como el usuario "canEditUser" para mostrar las capacidades adicionales que tendrá que agregó durante este tutorial. Para obtener detalles de API acerca de la identidad de ASP.NET, vea el [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Para obtener más información acerca de cómo inicializar el sistema de identidades de ASP.NET, vea el [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Restringir el acceso a la página de administración

La aplicación de ejemplo Wingtip Toys permite a los usuarios anónimos y los usuarios registrados ver y comprar productos. Sin embargo, el usuario que tiene el rol personalizado "canEdit" puede tener acceso a una página restringida para agregar y quitar productos.

#### <a name="add-an-administration-folder-and-page"></a>Agregar una carpeta de administración y la página

A continuación, creará una carpeta denominada *administración* aplicación de ejemplo para el usuario "canEditUser" que pertenece a la función personalizada de Wingtip Toys.

1. Haga clic en el nombre del proyecto (**Wingtip Toys**) en **el Explorador de soluciones** y seleccione **agregar**  - &gt; **nueva carpeta**.
2. Nombre de la nueva carpeta *administración*.
3. Haga clic en el *administración* carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
4. Seleccione el <strong>Visual C#</strong> - &gt; <strong>Web</strong> grupo de plantillas de la izquierda. En la lista central, seleccione <strong>formulario Web con página maestra</strong>, asígnele el nombre <em>AdminPage.aspx</em><strong>,</strong> y, a continuación, seleccione <strong>agregar</strong>.
5. Seleccione el *Site.Master* de archivos como la página maestra y, a continuación, elija **Aceptar**.

#### <a name="add-a-webconfig-file"></a>Agregue un archivo Web.config

Mediante la adición de un *Web.config* del archivo a la *administración* carpeta, puede restringir el acceso a la página de contenido en la carpeta.

1. Haga clic en el *administración* carpeta y seleccione **agregar**  - &gt; **nuevo elemento**.  
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. En la lista de plantillas web de Visual C#, seleccione <strong>archivo de configuración Web</strong>desde la lista en el medio, acepte el nombre predeterminado de <em>Web.config</em><strong>,</strong> y, a continuación, seleccione <strong>Agregar</strong>.
3. Reemplace el documento XML contenido en el *Web.config* archivo con lo siguiente:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Guardar el *Web.config* archivo. El *Web.config* archivo Especifica que sólo el usuario que pertenecen a la función "canEdit" de la aplicación puede tener acceso a la página contenida en el *administración* carpeta.

### <a name="including-custom-role-navigation"></a>Incluso en la exploración de roles personalizados

Para permitir al usuario de la función personalizada "canEdit" Ir a la sección Administración de la aplicación, debe agregar un vínculo a la *Site.Master* página. Solo los usuarios que pertenecen a la función "canEdit" podrán ver el **administración** vincular y tener acceso a la sección de administración.

1. En el Explorador de soluciones, busque y abra la *Site.Master* página.
2. Para crear un vínculo para el usuario de la función "canEdit", agregue el marcado que se resalta en amarillo en la siguiente lista sin ordenar `<ul>` elemento para que la lista aparece como sigue:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Abra la *Site.Master.cs* archivo. Realizar la **administración** vínculo visible solo para el usuario "canEditUser" agregando el código resaltado en color amarillo para los `Page_Load` controlador. El `Page_Load` controlador aparecerá como sigue:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Cuando se carga la página, el código comprueba si el usuario ha iniciado la sesión tiene el rol de "canEdit". Si el usuario pertenece a la función "canEdit", el elemento span que contiene el vínculo a la *AdminPage.aspx* página (y, por consiguiente, el vínculo en el intervalo) se hacen visibles.

### <a name="enabling-product-administration"></a>Habilitar la administración de producto

Hasta ahora, ha creado el rol "canEdit" y agrega un usuario "canEditUser", una carpeta de administración y una página de administración. Ha configurado los derechos de acceso para la carpeta de administración y la página y ha agregado un vínculo de navegación para el usuario de la función "canEdit" a la aplicación. A continuación, agregará el marcado para la *AdminPage.aspx* página y escribir código para el *AdminPage.aspx.cs* archivo de código subyacente que permita al usuario con el rol "canEdit" Agregar y quitar productos.

1. En **el Explorador de soluciones**, abra el *AdminPage.aspx* de archivos desde el *administración* carpeta.
2. Reemplace el marcado existente con lo siguiente:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. A continuación, abra el *AdminPage.aspx.cs* archivo de código subyacente haciendo clic en el *AdminPage.aspx* y haga clic en **ver código**.
4. Reemplace el código existente en el *AdminPage.aspx.cs* archivo de código subyacente por el código siguiente:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

En el código que ha especificado para la *AdminPage.aspx.cs* archivo de código subyacente, una clase denominada `AddProducts` realiza el trabajo real de agregar productos a la base de datos. Esta clase no existe aún, por lo que creará ahora.

1. En **el Explorador de soluciones**, haga clic en el *lógica* carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el **Visual C#**  - &gt; **código** grupo de plantillas de la izquierda. A continuación, seleccione **clase**desde la mitad de lista y asígnele el nombre *AddProducts.cs*.   
   Se muestra el nuevo archivo de clase.
3. Reemplace el código existente por el siguiente:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

El *AdminPage.aspx* página permite al usuario que pertenecen a la función "canEdit" Agregar y quitar productos. Cuando se agrega un nuevo producto, los detalles sobre el producto se validan y, a continuación, escritos en la base de datos. El nuevo producto está inmediatamente disponible para todos los usuarios de la aplicación web.

#### <a name="unobtrusive-validation"></a>Validación discreto

Los detalles del producto que proporciona el usuario en el *AdminPage.aspx* página se validan mediante controles de validación (`RequiredFieldValidator` y `RegularExpressionValidator`). Estos controles automáticamente mediante la validación discreto. Validación discreto permite que los controles de validación que se va a usar JavaScript para la lógica de validación del lado cliente, lo que significa que la página no requiere un viaje en el servidor que se debe validar. De forma predeterminada, la validación discreto se incluye en el *Web.config* basado en archivos en la opción de configuración siguiente:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Expresiones regulares

El precio del producto en el *AdminPage.aspx* página se valida mediante una **RegularExpressionValidator** control. Este control se valida si el valor del control de entrada asociado (el cuadro de texto "AddProductPrice") coincide con el patrón especificado por la expresión regular. Una expresión regular es una notación de coincidencia de patrones que le permite encontrar rápidamente y deben coincidir con patrones de caracteres específica. El **RegularExpressionValidator** control incluye una propiedad denominada `ValidationExpression` que contiene la expresión regular utilizada para validar los datos proporcionados por precio, tal y como se muestra a continuación:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Control fileUpload

Además de los controles de entrada y validación, agrega el **FileUpload** el control a la *AdminPage.aspx* página. Este control proporciona la capacidad para cargar archivos. En este caso, solo se permite al cargar los archivos de imagen. En el archivo de código subyacente (*AdminPage.aspx.cs*), cuando la `AddProductButton` se hace clic, el código comprueba el `HasFile` propiedad de la **FileUpload** control. Si el control tiene un archivo y si se permite el tipo de archivo (en función de extensión de archivo), la imagen se guarda en el *imágenes* carpeta y el *imágenes/Pulgar hacia* carpeta de la aplicación.

#### <a name="model-binding"></a>Enlace de modelos

Anteriormente en esta serie de tutoriales usa el enlace de modelos para rellenar un **ListView** (control), un **FormsView** (control), un **GridView** (control) y un  **DetailView** control. En este tutorial, use el enlace de modelos para rellenar un **DropDownList** control con una lista de categorías de productos.

El marcado que agrega a la *AdminPage.aspx* archivo contiene un **DropDownList** control denominado `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Usar el enlace de modelos para rellenar este **DropDownList** estableciendo la `ItemType` atributo y el `SelectMethod` atributo. El `ItemType` atributo especifica que se use la `WingtipToys.Models.Category` escriba al rellenar el control. Define este tipo al principio de esta serie de tutoriales creando el `Category` clase (que se muestra a continuación). El `Category` clase está en el *modelos* carpeta dentro de la *Category.cs* archivo.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

El `SelectMethod` atributo de la **DropDownList** control especifica que se use la `GetCategories` (método) (se muestra a continuación) que se incluye en el archivo de código subyacente (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Este método especifica que un `IQueryable` interfaz se usa para evaluar una consulta en un `Category` tipo. El valor devuelto se usa para rellenar el **DropDownList** en el marcado de la página (*AdminPage.aspx*).

El texto mostrado para cada elemento de la lista se especifica estableciendo el `DataTextField` atributo. El `DataTextField` atributo usa el `CategoryName` de la `Category` clase (que se muestra anteriormente) para mostrar cada categoría en la **DropDownList** control. El valor real que se pasa cuando se selecciona un elemento en el **DropDownList** control se basa en el `DataValueField` atributo. El `DataValueField` atributo está establecido en el `CategoryID` tal y como se definen en la `Category` clase (que se muestra arriba).

### <a name="how-the-application-will-work"></a>El funcionamiento de la aplicación

Cuando el usuario que pertenecen a la función "canEdit" navega a la página por primera vez, el `DropDownAddCategory` **DropDownList** se rellena como se describió anteriormente. El `DropDownRemoveProduct` **DropDownList** control también se rellena con los productos con el mismo enfoque. El usuario que pertenecen a la función "canEdit" selecciona el tipo de categoría y agrega los detalles del producto (**nombre**, **descripción**, **precio**, y **delarchivodeimagen**). Cuando hace clic en el usuario que pertenezca a la función "canEdit" el **agregar producto** botón, el `AddProductButton_Click` se activa el controlador de eventos. El `AddProductButton_Click` controlador de eventos se encuentra en el archivo de código subyacente (*AdminPage.aspx.cs*) comprueba el archivo de imagen para asegurarse de que coincide con los tipos de archivo permitidos *(.gif*, *.png*, *.jpeg*, o *.jpg*). A continuación, se guarda el archivo de imagen en una carpeta de la aplicación de ejemplo Wingtip Toys. A continuación, se agrega el nuevo producto a la base de datos. Para realizar la adición de un nuevo producto, una nueva instancia de la `AddProducts` clase está creada y denominada productos. El `AddProducts` clase tiene un método denominado `AddProduct`, y el objeto de productos llama a este método para agregar productos a la base de datos.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Si el código agrega correctamente el nuevo producto a la base de datos, se vuelve a cargar la página con el valor de cadena de consulta `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Cuando vuelve a cargar la página, la cadena de consulta se incluye en la dirección URL. Volviendo a cargar la página, el usuario que pertenezca a la función "canEdit" pueda ver inmediatamente las actualizaciones en el **DropDownList** a los controles de la *AdminPage.aspx* página. Además, mediante la inclusión de la cadena de consulta con la dirección URL, la página puede mostrar un mensaje de confirmación al usuario que pertenecen a la función "canEdit".

Cuando el *AdminPage.aspx* página recargas más rápidas, la `Page_Load` se llama al evento.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

El `Page_Load` controlador de eventos comprueba el valor de cadena de consulta y determina si se debe mostrar un mensaje de confirmación.

## <a name="running-the-application"></a>Ejecutar la aplicación

Puede ejecutar la aplicación ahora para ver cómo se pueden agregar, eliminar y actualiza los artículos en el carro de la compra. El total de carro de la compra reflejará el costo total de todos los elementos en el carro de la compra.

1. En el Explorador de soluciones, presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
   El explorador se abre y muestra el *Default.aspx* página.
2. Haga clic en el **sesión** vínculo en la parte superior de la página. 

    ![Pertenencia y administración - registro de vínculo](membership-and-administration/_static/image2.png)

   El *Login.aspx* se muestra la página.
3. Utilice el siguiente nombre de usuario y la contraseña:  
   Nombre de usuario: canEditUser@wingtiptoys.com  
   Contraseña: Pa$ $word1 

    ![Pertenencia y administración - iniciar sesión en la página](membership-and-administration/_static/image3.png)
4. Haga clic en el **sesión** botón situado cerca de la parte inferior de la página.
5. En la parte superior de la página siguiente, seleccione la **administración** vínculo para navegar a la *AdminPage.aspx* página. 

    ![Pertenencia y administración - vínculo de administración](membership-and-administration/_static/image4.png)
6. Para probar la validación de entrada, haga clic en el **agregar producto** botón sin tener que agregar los detalles del producto. 

    ![Pertenencia y administración - página de administración](membership-and-administration/_static/image5.png)

   Tenga en cuenta que se muestran los mensajes de campo obligatorio.
7. Agregue los detalles de un nuevo producto y, a continuación, haga clic en el **agregar producto** botón. 

    ![Pertenencia y administración - agregar producto](membership-and-administration/_static/image6.png)
8. Seleccione **productos** desde el menú de navegación superior para ver el nuevo producto que agregó. 

    ![Pertenencia y administración - mostrar el nuevo producto](membership-and-administration/_static/image7.png)
9. Haga clic en el **administrador** para volver a la página de administración.
10. En el **quitar producto** sección de la página, seleccione el nuevo producto que agregó en el **DropDownListBox**.
11. Haga clic en el **quitar producto** botón para quitar el nuevo producto de la aplicación. 

    ![Pertenencia y administración - Remove producto](membership-and-administration/_static/image8.png)
12. Seleccione **productos** en el menú de navegación superior para confirmar que se ha quitado el producto.
13. Haga clic en **cerrar** que existe el modo de administración.   
    Tenga en cuenta que el panel de navegación superior ya no muestra la **administración** elemento de menú.

## <a name="summary"></a>Resumen

En este tutorial, ha agregado un rol personalizado y un usuario que pertenezca a la función personalizada, acceso restringido a la carpeta de administración y la página y proporciona métodos de navegación para el usuario que pertenezca a la función personalizada. Enlace de modelos se utilizó para llenar un **DropDownList** control con datos. Se implementa el **FileUpload** control y los controles de validación. Además, ha aprendido cómo agregar y quitar los productos de una base de datos. En el siguiente tutorial, aprenderá a implementar el enrutamiento de ASP.NET.

## <a name="additional-resources"></a>Recursos adicionales

[Web.config - elemento de autorización](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[Identidad de ASP.NET](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Implementar una aplicación de formularios Web de ASP.NET segura con pertenencia, OAuth y base de datos SQL en un sitio Web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versión de prueba gratuita](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](checkout-and-payment-with-paypal.md)
> [Siguiente](url-routing.md)
