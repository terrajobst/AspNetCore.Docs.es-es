---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Pertenencia y administración | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 58eda260ccf2d0f237aaade65017760ed87a8c4f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368303"
---
<a name="membership-and-administration"></a>Pertenencia y administración
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible para acompañar esta serie de tutoriales.


Este tutorial muestra cómo actualizar la aplicación de ejemplo Wingtip Toys para agregar un rol personalizado y usar ASP.NET Identity. También se muestra cómo implementar una página de administración desde el que el usuario con un rol personalizado puede agregar y quitar productos desde el sitio Web.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) es el sistema de pertenencia que se usa para compilar la aplicación web ASP.NET y está disponible en ASP.NET 4.5. ASP.NET Identity se utiliza en la plantilla de proyecto de formularios Web Forms de Visual Studio 2013, así como las plantillas para [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), y [aplicación de página única de ASP.NET](../../../../single-page-application/index.md). Específicamente, puede instalar el sistema ASP.NET Identity mediante NuGet cuando se inicia con una aplicación Web vacía. Sin embargo, en esta serie de tutoriales utilice el **formularios Web Forms**projecttemplate, que incluye el sistema ASP.NET Identity. ASP.NET Identity facilita integrar datos de perfil de usuario específica con datos de la aplicación. Además, ASP.NET Identity permite elegir el modelo de persistencia para perfiles de usuario en la aplicación. Puede almacenar los datos en una base de datos de SQL Server o en otro almacén de datos, incluidos *NoSQL* almacenes de datos como tablas de almacenamiento de Windows Azure.

Este tutorial se basa en el tutorial anterior titulado "Desprotección y pago con PayPal" en la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo usar código para agregar un rol personalizado y un usuario a la aplicación.
- Cómo restringir el acceso a la carpeta de administración y la página.
- Cómo proporcionar navegación para el usuario que pertenezca al rol personalizado.
- Cómo usar el enlace de modelos para rellenar un [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) control con las categorías de productos.
- Cómo cargar un archivo en la aplicación web con el [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) control.
- Cómo usar los controles de validación para implementar la validación de entrada.
- Cómo agregar y quitar los productos de la aplicación.

## <a name="these-features-are-included-in-the-tutorial"></a>Estas características se incluyen en el tutorial:

- ASP.NET Identity
- Configuración y la autorización
- Enlace de modelos
- Validación discreta

ASP.NET Web Forms proporciona capacidades de pertenencia. Mediante el uso de la plantilla predeterminada, tiene la funcionalidad de pertenencia integrado que puede utilizar inmediatamente cuando se ejecuta la aplicación. Este tutorial muestra cómo usar ASP.NET Identity para agregar un rol personalizado y asignar a un usuario a ese rol. Obtendrá información sobre cómo restringir el acceso a la carpeta de administración. Agregará una página a la carpeta de administración que permite a un usuario con un rol personalizado para agregar y quitar los productos y obtener una vista previa de un producto después de que se ha agregado.

## <a name="adding-a-custom-role"></a>Agregar un rol personalizado

Con ASP.NET Identity, puede agregar un rol personalizado y asignar a un usuario a ese rol usando código.

1. En **el Explorador de soluciones**, haga doble clic en el *lógica* carpeta y cree una nueva clase.
2. Nombre de la nueva clase *RoleActions.cs*.
3. Modifique el código para que aparezca como sigue:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. En **el Explorador de soluciones**, abra el *Global.asax.cs* archivo.
5. Modificar el *Global.asax.cs* archivo agregando el código resaltado en amarillo para que aparezca como sigue:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Tenga en cuenta que `AddUserAndRole` está subrayada en rojo. Haga doble clic en el código AddUserAndRole.  
   La letra "A" al principio del método resaltado aparecerá subrayada.
7. Mantenga el mouse sobre la letra "A" y haga clic en la interfaz de usuario que le permite generar código auxiliar de método para el `AddUserAndRole` método. 

    ![Pertenencia y Advministration - generar código auxiliar del método](membership-and-administration/_static/image1.png)
8. Haga clic en la opción titulada:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Abra el *RoleActions.cs* de archivos desde el *lógica* carpeta.  
   El `AddUserAndRole` método se ha agregado al archivo de clase.
10. Modificar el *RoleActions.cs* archivo quitando la `NotImplementedeException` y agregue el código resaltado en amarillo, para que aparezca como sigue:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Primero, el código anterior establece un contexto de base de datos para la base de datos de pertenencia. También se almacena la base de datos de pertenencia como una *.mdf* de archivos en el *aplicación\_datos* carpeta. Podrá ver esta base de datos una vez que el primer usuario ha iniciado sesión en esta aplicación web. 

> [!NOTE] 
> 
> Si desea almacenar los datos de pertenencia, junto con los datos del producto, puede usar el mismo **DbContext** que ha usado para almacenar los datos del producto en el código anterior.


 El *interno* palabra clave es un modificador de acceso para miembros de tipo (por ejemplo, métodos o propiedades) y tipos (por ejemplo, las clases). Tipos internos o los miembros son accesibles solo dentro de los archivos contenidos en el mismo ensamblado *(.dll* archivo). Al compilar la aplicación, un archivo de ensamblado *(.dll*) se crea que contiene el código que se ejecuta cuando se ejecute la aplicación. 

Un `RoleStore` object, que proporciona administración de roles, se crea basándose en el contexto de base de datos.

> [!NOTE] 
> 
> Tenga en cuenta que, cuando el `RoleStore` se crea el objeto usa un tipo genérico `IdentityRole` tipo. Esto significa que el `RoleStore` solo puede contener `IdentityRole` objetos. También mediante el uso de genéricos, se administran mejor los recursos de memoria.


A continuación, el `RoleManager` de objetos, se crea basándose en la `RoleStore` objeto que acaba de crear. el `RoleManager` objeto expone funciones relacionadas con la API que se puede usar para guardar automáticamente los cambios realizados en el `RoleStore`. El `RoleManager` solo puede contener `IdentityRole` objetos porque el código utiliza el `<IdentityRole>` tipo genérico.

Se llama a la `RoleExists` método para determinar si el rol "canEdit" está presente en la base de datos de pertenencia. Si no es así, cree el rol.

Crear el `UserManager` objeto parece ser más complicado que el `RoleManager` controlar, sin embargo es prácticamente el mismo. Solo se codifica en una línea en lugar de varias. En este caso, el parámetro que está pasando está creando instancias como un nuevo objeto contenido en los paréntesis.

A continuación cree el usuario "canEditUser" mediante la creación de un nuevo `ApplicationUser` objeto. A continuación, si consigue crear el usuario, agregue el usuario al nuevo rol.

> [!NOTE] 
> 
> El control de errores se actualizará durante el tutorial "Control de errores de ASP.NET" más adelante en esta serie de tutoriales.


La próxima vez que se inicia la aplicación, se agregará el usuario denominado "canEditUser" como la función denominada "canEdit" de la aplicación. Más adelante en este tutorial, iniciará sesión como el usuario "canEditUser" para mostrar las capacidades adicionales que se agregan durante este tutorial. Para obtener detalles de API acerca de la identidad de ASP.NET, vea el [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Para obtener más información acerca de cómo inicializar el sistema ASP.NET Identity, vea el [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Restringir el acceso a la página de administración

La aplicación de ejemplo Wingtip Toys permite a los usuarios anónimos y los usuarios registrados ver y comprar productos. Sin embargo, el usuario que tenga el rol "canEdit" personalizado puede tener acceso a una página restringida con el fin de agregar y quitar los productos.

#### <a name="add-an-administration-folder-and-page"></a>Agregar una carpeta de administración y la página

A continuación, creará una carpeta denominada *Admin* aplicación de ejemplo para el usuario "canEditUser" que pertenecen al rol personalizado de la serie de Wingtip Toys.

1. Haga clic en el nombre del proyecto (**Wingtip Toys**) en **el Explorador de soluciones** y seleccione **agregar**  - &gt; **nueva carpeta**.
2. Nombre de la nueva carpeta *Admin*.
3. Haga clic en el *Admin* carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
4. Seleccione el <strong>Visual C#</strong> - &gt; <strong>Web</strong> grupo de plantillas de la izquierda. En la lista central, seleccione <strong>formulario Web Forms con página maestra</strong>, asígnele el nombre <em>AdminPage.aspx</em><strong>,</strong> y, a continuación, seleccione <strong>agregar</strong>.
5. Seleccione el *Site.Master* de archivos como la página maestra y, a continuación, elija **Aceptar**.

#### <a name="add-a-webconfig-file"></a>Agregue un archivo Web.config

Agregando un *Web.config* del archivo a la *Admin* carpeta, puede restringir el acceso a la página de contenidos en la carpeta.

1. Haga clic en el *Admin* carpeta y seleccione **agregar**  - &gt; **nuevo elemento**.  
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. En la lista de plantillas web de Visual C#, seleccione <strong>archivo de configuración Web</strong>desde la lista del medio, acepte el nombre predeterminado de <em>Web.config</em><strong>,</strong> y, a continuación, seleccione <strong>Agregar</strong>.
3. Reemplace el contenido XML del *Web.config* archivo por lo siguiente:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Guardar el *Web.config* archivo. El *Web.config* archivo Especifica que sólo el usuario que pertenecen al rol "canEdit" de la aplicación puede tener acceso a la página de contenido en el *Admin* carpeta.

### <a name="including-custom-role-navigation"></a>Incluida la navegación del rol personalizado

Para permitir al usuario del rol "canEdit" personalizada navegar hasta la sección de administración de la aplicación, debe agregar un vínculo a la *Site.Master* página. Solo los usuarios que pertenecen al rol "canEdit" podrán ver el **Admin** vincular y tener acceso a la sección de administración.

1. En el Explorador de soluciones, busque y abra el *Site.Master* página.
2. Para crear un vínculo para el usuario del rol "canEdit", agregue el marcado resaltado en amarillo en la siguiente lista sin ordenar `<ul>` elemento para que aparezca la lista como sigue:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Abra el *Site.Master.cs* archivo. Realizar el **Admin** vínculo solo está visible para el usuario "canEditUser" agregando el código resaltado en amarillo en la `Page_Load` controlador. El `Page_Load` controlador aparecerá como sigue:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Cuando se carga la página, el código comprueba si el usuario ha iniciado sesión tiene el rol de "canEdit". Si el usuario pertenece al rol "canEdit", el elemento span que contiene el vínculo a la *AdminPage.aspx* página (y por consiguiente el vínculo dentro del intervalo) se hacen visibles.

### <a name="enabling-product-administration"></a>Habilitar la administración de productos

Hasta ahora, ha creado el rol "canEdit" y agregar un usuario "canEditUser", una carpeta de administración y una página de administración. Se han establecido derechos de acceso de la carpeta de administración y la página y ha agregado un vínculo de navegación para el usuario del rol "canEdit" a la aplicación. A continuación, agregará código para el *AdminPage.aspx* página y el código para el *AdminPage.aspx.cs* archivo de código subyacente que permitirá al usuario con el rol "canEdit" Agregar y quitar los productos.

1. En **el Explorador de soluciones**, abra el *AdminPage.aspx* de archivos desde el *Admin* carpeta.
2. Reemplace el marcado existente por lo siguiente:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. A continuación, abra el *AdminPage.aspx.cs* archivo de código subyacente con el botón secundario del *AdminPage.aspx* y haga clic en **ver código**.
4. Reemplace el código existente en el *AdminPage.aspx.cs* archivo de código subyacente con el código siguiente:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

En el código que escribió para el *AdminPage.aspx.cs* archivo de código subyacente, una clase denominada `AddProducts` realiza el trabajo real de agregar productos a la base de datos. Esta clase no existe aún, por lo que creará ahora.

1. En **el Explorador de soluciones**, haga clic en el *lógica* carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el **Visual C#**  - &gt; **código** grupo de plantillas de la izquierda. A continuación, seleccione **clase**desde la parte central de lista y asígnele el nombre *AddProducts.cs*.   
   Se muestra el nuevo archivo de clase.
3. Reemplace el código existente por el siguiente:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

El *AdminPage.aspx* página permite al usuario que pertenecen al rol "canEdit" Agregar y quitar los productos. Cuando se agrega un nuevo producto, los detalles sobre el producto se validan y, a continuación, introducidos en la base de datos. El nuevo producto está inmediatamente disponible para todos los usuarios de la aplicación web.

#### <a name="unobtrusive-validation"></a>Validación discreta

Los detalles del producto que proporciona el usuario en el *AdminPage.aspx* página se validan mediante los controles de validación (`RequiredFieldValidator` y `RegularExpressionValidator`). Estos controles usa automáticamente la validación discreta. Validación discreta permite usar JavaScript para la lógica de validación del lado cliente, lo que significa que la página no requiere un viaje en el servidor para validar los controles de validación. De forma predeterminada, se incluye la validación discreta en el *Web.config* basado en archivos en la siguiente configuración:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Expresiones regulares

El precio del producto en el *AdminPage.aspx* página se valida con un **RegularExpressionValidator** control. Este control se valida si el valor del control de entrada asociado (el cuadro de texto "AddProductPrice") coincide con el patrón especificado por la expresión regular. Una expresión regular es una notación de coincidencia de patrones que le permite encontrar rápidamente y coinciden con patrones de caracteres específicos. El **RegularExpressionValidator** control incluye una propiedad denominada `ValidationExpression` que contiene la expresión regular utilizada para validar la entrada de precio, como se muestra a continuación:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Control fileUpload

Además de los controles de entrada y validación, agregó el **FileUpload** el control a la *AdminPage.aspx* página. Este control proporciona la capacidad para cargar archivos. En este caso, solo se permite cargar los archivos de imagen. En el archivo de código subyacente (*AdminPage.aspx.cs*), cuando el `AddProductButton` se hace clic en, el código comprueba el `HasFile` propiedad de la **FileUpload** control. Si el control tiene un archivo y si se permite el tipo de archivo (según la extensión de archivo), la imagen se guarda en el *imágenes* carpeta y el *imágenes/pulgares* carpeta de la aplicación.

#### <a name="model-binding"></a>Enlace de modelos

Anteriormente en esta serie de tutoriales se usa el enlace de modelos para rellenar un **ListView** (control), un **FormsView** (control), un **GridView** control y un  **DetailView** control. En este tutorial, use el enlace de modelos para rellenar un **DropDownList** control con una lista de categorías de productos.

El marcado que se ha agregado a la *AdminPage.aspx* archivo contiene un **DropDownList** control denominado `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Usar el enlace de modelos para rellenar esta **DropDownList** estableciendo el `ItemType` atributo y el `SelectMethod` atributo. El `ItemType` atributo especifica que se usa el `WingtipToys.Models.Category` escriba al rellenar el control. Este tipo al principio de esta serie de tutoriales se define mediante la creación del `Category` clase (se muestra a continuación). El `Category` clase está en el *modelos* carpeta dentro de la *Category.cs* archivo.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

El `SelectMethod` atributo de la **DropDownList** control especifica que se usa el `GetCategories` método (que se muestra a continuación) que es incluido en el archivo de código subyacente (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Este método especifica que un `IQueryable` interfaz se usa para evaluar una consulta en un `Category` tipo. El valor devuelto se usa para rellenar el **DropDownList** en el marcado de la página (*AdminPage.aspx*).

El texto mostrado para cada elemento de la lista se especifica estableciendo el `DataTextField` atributo. El `DataTextField` atributo usa el `CategoryName` de la `Category` clase (mostrado anteriormente) para mostrar cada categoría en la **DropDownList** control. El valor real que se pasa cuando se selecciona un elemento en el **DropDownList** control se basa en el `DataValueField` atributo. El `DataValueField` atributo está establecido en el `CategoryID` tal y como se definen en el `Category` clase (mostrado anteriormente).

### <a name="how-the-application-will-work"></a>Funcionamiento de la aplicación

Cuando el usuario que pertenecen al rol "canEdit" navega a la página por primera vez, el `DropDownAddCategory` **DropDownList** se rellena el control como se describió anteriormente. El `DropDownRemoveProduct` **DropDownList** control también se rellena con los productos con el mismo enfoque. El usuario que pertenezca al rol "canEdit" selecciona el tipo de categoría y agrega los detalles del producto (**nombre**, **descripción**, **precio**, y **delarchivodeimagen**). Cuando hace clic en el usuario que pertenezca al rol "canEdit" el **agregar producto** botón, el `AddProductButton_Click` se activa el controlador de eventos. El `AddProductButton_Click` controlador de eventos que se encuentra en el archivo de código subyacente (*AdminPage.aspx.cs*) comprueba el archivo de imagen para asegurarse de que coincida con los tipos de archivo permitidos *(.gif*, *.png*, *.jpeg*, o *.jpg*). A continuación, se guarda el archivo de imagen en una carpeta de la aplicación de ejemplo Wingtip Toys. A continuación, se agrega el nuevo producto a la base de datos. Para agregar un nuevo producto, una nueva instancia de la `AddProducts` clase se crea y denominada products. El `AddProducts` clase tiene un método denominado `AddProduct`, y el objeto de productos, llama a este método para agregar productos a la base de datos.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Si el código agrega correctamente el nuevo producto a la base de datos, se vuelve a cargar la página con el valor de cadena de consulta `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Cuando vuelve a cargar la página, se incluye la cadena de consulta en la dirección URL. Volviendo a cargar la página, el usuario que pertenezca al rol "canEdit" puede ver inmediatamente las actualizaciones en el **DropDownList** a los controles de la *AdminPage.aspx* página. Además, mediante la inclusión de la cadena de consulta con la dirección URL, la página puede mostrar un mensaje de confirmación al usuario que pertenecen al rol "canEdit".

Cuando el *AdminPage.aspx* página recargas, el `Page_Load` se llama al evento.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

El `Page_Load` controlador de eventos comprueba el valor de cadena de consulta y determina si se debe mostrar un mensaje de confirmación.

## <a name="running-the-application"></a>Ejecutar la aplicación

Puede ejecutar la aplicación ahora para ver cómo se puede agregar, eliminar y actualizar elementos en el carro de la compra. El total del carro de la compra reflejará el costo total de todos los elementos en el carro de la compra.

1. En el Explorador de soluciones, presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
   El explorador se abre y muestra el *Default.aspx* página.
2. Haga clic en el **iniciarla** vínculo en la parte superior de la página. 

    ![Pertenencia y administración - inicie sesión en el vínculo](membership-and-administration/_static/image2.png)

   El *Login.aspx* se muestra la página.
3. Use el siguiente nombre de usuario y la contraseña:  
   Nombre de usuario: canEditUser@wingtiptoys.com  
   Contraseña: Pa$ $word1 

    ![Pertenencia y administración - inicie sesión en la página](membership-and-administration/_static/image3.png)
4. Haga clic en el **iniciarla** situado cerca de la parte inferior de la página.
5. En la parte superior de la página siguiente, seleccione el **Admin** vínculo para ir a la *AdminPage.aspx* página. 

    ![Pertenencia y administración - vínculo de administración](membership-and-administration/_static/image4.png)
6. Para probar la validación de entrada, haga clic en el **agregar producto** botón sin agregar los detalles del producto. 

    ![Pertenencia y administración - página de administración](membership-and-administration/_static/image5.png)

   Tenga en cuenta que se muestran los mensajes de campo obligatorio.
7. Agregue los detalles de un producto nuevo y, a continuación, haga clic en el **agregar producto** botón. 

    ![Pertenencia y administración - agregar producto](membership-and-administration/_static/image6.png)
8. Seleccione **productos** desde el menú de navegación superior para ver el nuevo producto que agregó. 

    ![Pertenencia y administración - mostrar el nuevo producto](membership-and-administration/_static/image7.png)
9. Haga clic en el **administrador** para volver a la página de administración.
10. En el **quitar producto** sección de la página, seleccione el nuevo producto que agregó en el **DropDownListBox**.
11. Haga clic en el **quitar producto** botón para quitar el nuevo producto de la aplicación. 

    ![Pertenencia y administración - Remove producto](membership-and-administration/_static/image8.png)
12. Seleccione **productos** en el menú de navegación superior para confirmar que el producto se ha quitado.
13. Haga clic en **cerrar** que exista el modo de administración.   
    Tenga en cuenta que el panel de navegación superior ya no muestra la **Admin** elemento de menú.

## <a name="summary"></a>Resumen

En este tutorial, ha agregado un rol personalizado y un usuario que pertenezca al rol personalizado, acceso restringido a la carpeta de administración y la página y proporciona navegación para el usuario que pertenezca al rol personalizado. Usa el enlace de modelos para rellenar un **DropDownList** control con datos. Se implementa el **FileUpload** control y los controles de validación. Además, ha aprendido a agregar y quitar los productos de una base de datos. En el siguiente tutorial, obtendrá información sobre cómo implementar el enrutamiento de ASP.NET.

## <a name="additional-resources"></a>Recursos adicionales

[Web.config - elemento authorization](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Implementar una aplicación de formularios Web ASP.NET segura con pertenencia, OAuth y SQL Database en un sitio Web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](checkout-and-payment-with-paypal.md)
> [Siguiente](url-routing.md)
