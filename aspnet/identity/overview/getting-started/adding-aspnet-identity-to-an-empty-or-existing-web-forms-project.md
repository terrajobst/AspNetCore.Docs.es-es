---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Agregar proyecto de formularios de ASP.NET Identity a un Web vacío o existente | Microsoft Docs
author: raquelsa
description: Este tutorial muestra cómo agregar ASP.NET Identity (el nuevo sistema de pertenencia de ASP.NET) a una aplicación ASP.NET. Cuando se crea un nuevo formularios Web Forms o MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: cd28cc68db96b52eb205b8764aa2af014ffad9c3
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236515"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Agregar ASP.NET Identity a una existente o vacío Web Forms Project


> Este tutorial muestra cómo agregar [ASP.NET Identity](introduction-to-aspnet-identity.md) (el nuevo sistema de pertenencia de ASP.NET) a una aplicación ASP.NET.
> 
> Al crear un nuevo proyecto de formularios Web Forms o MVC en Visual Studio 2017 RTM con cuentas individuales, Visual Studio instalará todos los paquetes necesarios y agregar todas las clases necesarias para usted. En este tutorial se explica los pasos para agregar compatibilidad de ASP.NET Identity en el proyecto de formularios Web Forms existente o un proyecto vacío nuevo. Describo todos los paquetes NuGet que necesita para instalar y clases que necesarias para agregar. Pasará a través de formularios Web Forms de ejemplo para el registro de nuevos usuarios e iniciar sesión mientras se resalta todas las API de punto de entrada principal para la autenticación y administración de usuarios. Este ejemplo usará la implementación predeterminada de ASP.NET Identity para el almacenamiento de datos SQL que se basa en Entity Framework. En este tutorial, usaremos LocalDB para la base de datos SQL.
> 

## <a name="get-started-with-aspnet-identity"></a>Introducción a ASP.NET Identity

1. Comience por instalar y ejecutar [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Seleccione **nuevo proyecto** desde el principio página, o puede usar el menú y seleccione **archivo**y, a continuación, **nuevo proyecto**.
3. En el panel izquierdo, expanda **Visual C#** , a continuación, seleccione **Web**, a continuación, **aplicación Web ASP.NET (.Net Framework)**. Nombre del proyecto "WebFormsIdentity" y seleccione **Aceptar**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **vacía** plantilla.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Tenga en cuenta la **Cambiar autenticación** botón está deshabilitado y no hay compatibilidad con la autenticación se proporciona en esta plantilla. Las plantillas de formularios Web Forms, MVC y Web API permiten seleccionar el método de autenticación.

## <a name="add-identity-packages-to-your-app"></a>Agregar paquetes de identidad a la aplicación

En el Explorador de soluciones, haga clic en el proyecto y seleccione **administrar paquetes de NuGet**. Busque e instale el **Microsoft.AspNet.Identity.EntityFramework** paquete. 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Tenga en cuenta que este paquete se instalará los paquetes de dependencia: **EntityFramework** y **Microsoft Identity de ASP.NET Core**.

## <a name="add-a-web-form-to-register-users"></a>Agregue un formulario web para registrar los usuarios

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar**y, a continuación, **formulario Web Forms**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. En el **especificar nombre para el elemento** cuadro de diálogo, el nombre del nuevo formulario web **registrar**y, a continuación, seleccione **Aceptar**
3. Reemplace el marcado generado *Register.aspx* archivo con el código siguiente. Los cambios de código aparecen resaltados. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Esto es simplemente una versión simplificada de la *Register.aspx* archivo que se crea cuando se crea un nuevo proyecto de formularios Web Forms de ASP.NET. El código anterior agrega campos de formulario y un botón para registrar un nuevo usuario.
4. Abra el *Register.aspx.cs* de archivo y reemplace el contenido del archivo con el código siguiente:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. El código anterior es una versión simplificada de la *Register.aspx.cs* archivo que se crea cuando se crea un nuevo proyecto de formularios Web Forms de ASP.NET.
    > 2. El *IdentityUser* clase es la implementación de EntityFramework predeterminada de la *IUser* interfaz. *IUser* es la interfaz mínima para un usuario de la identidad de ASP.NET Core.
    > 3. El *UserStore* clase es la implementación de EntityFramework predeterminada de un almacén de usuario. Esta clase implementa interfaces mínimas del núcleo de ASP.NET Identity: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* y *IUserRoleStore*.
    > 4. El *UserManager* clase expone usuario relacionados con las API que se va a guardar automáticamente los cambios realizados en el *UserStore*.
    > 5. El *IdentityResult* clase representa el resultado de una operación de identidad.
5. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar**, **Agregar carpeta ASP.NET** y, a continuación, **aplicación\_datos**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Abra el *Web.config* archivo y agregue una entrada de cadena de conexión para la base de datos que se usará para almacenar información de usuario. La base de datos se crearán en tiempo de ejecución por Entity Framework para las entidades de identidad. La cadena de conexión es similar a la que se crea automáticamente cuando se crea un nuevo proyecto de formularios Web Forms. El código resaltado se muestra el marcado, debe agregar:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Para Visual Studio 2015 o posterior, reemplace `(localdb)\v11.0` con `(localdb)\MSSQLLocalDB` en la cadena de conexión.
    
7. A la derecha, haga clic en archivo *Register.aspx* en el proyecto y seleccione **establecer como página de inicio**. Presione Ctrl + F5 para compilar y ejecutar la aplicación web. Escriba un nuevo nombre de usuario y contraseña y, a continuación, seleccione **registrar**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity tiene compatibilidad para la validación y en este ejemplo, puede comprobar el comportamiento predeterminado del usuario y contraseña validadores que proceden de paquete principal de identidad. El validador predeterminado para el usuario (`UserValidator`) tiene una propiedad `AllowOnlyAlphanumericUserNames` que tiene el valor predeterminado establecido en `true`. El validador predeterminado para la contraseña (`MinimumLengthValidator`) garantiza que la contraseña tiene al menos 6 caracteres. Estos validadores son propiedades en `UserManager` que se puede invalidar si desea tener validación personalizada,

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Compruebe la base de datos de identidad de LocalDb y tablas generadas por Entity Framework

1. En el **vista** menú, seleccione **Explorador de servidores**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Expanda **DefaultConnection (WebFormsIdentity)**, expanda **tablas**, haga clic en **AspNetUsers** y, a continuación, seleccione **mostrar datos de tabla**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Configurar la aplicación para la autenticación OWIN

En este momento sólo hemos agregado compatibilidad para la creación de usuarios. Ahora, vamos a demostrar cómo podemos agregar autenticación para iniciar sesión un usuario. ASP.NET Identity utiliza autenticación de Microsoft OWIN middleware para la autenticación de formularios. La autenticación de cookies de OWIN es una cookie y reclama el mecanismo de autenticación basada en que se puede usar cualquier marco de trabajo hospedados en [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) o IIS. Con este modelo, se pueden usar los mismos paquetes de autenticación en varios marcos, incluido ASP.NET MVC y Web Forms. Para obtener más información sobre el proyecto Katana y cómo ejecutar middleware en un host independiente, vea [introducción con el proyecto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Instalar paquetes de autenticación a la aplicación

1. En el Explorador de soluciones, haga clic en el proyecto y seleccione **administrar paquetes de NuGet**. Busque e instale el ***Microsoft.AspNet.Identity.Owin*** paquete. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Busque e instale el ***Microsoft.Owin.Host.SystemWeb*** paquete.

    > [!NOTE]
    > El **Microsoft.Aspnet.Identity.Owin** paquete contiene un conjunto de clases de extensión OWIN para administrar y configurar el middleware de autenticación OWIN a ser consumidos por los paquetes de la identidad de ASP.NET Core.
    > El **Microsoft.Owin.Host.SystemWeb** paquete contiene un servidor OWIN que habilita las aplicaciones basado en OWIN se ejecuten en IIS mediante la canalización de solicitudes ASP.NET. Para obtener más información, consulte [Middleware de OWIN en IIS integrado canalización](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Agregar clases de configuración de inicio y la autenticación OWIN

1. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar**y, a continuación, **Agregar nuevo elemento**. En el cuadro de diálogo de cuadro de texto de búsqueda, escriba "*owin*". La clase el nombre "*inicio*" y seleccione **agregar**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. En el archivo Startup.cs, agregue el código resaltado que se muestra a continuación para configurar la autenticación de cookies OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Esta clase contiene la `OwinStartup` atributo para especificar la clase de inicio OWIN. Todas las aplicaciones OWIN tiene una clase de inicio donde se especifican los componentes de la canalización de aplicación. Consulte [detección de clase de inicio OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) para obtener más información sobre este modelo.

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Agregar formularios web para registrar e iniciar sesión a los usuarios

1. Abra el *Register.aspx.cs* archivo y agregue el siguiente código que inicia sesión el usuario cuando el registro es satisfactorio.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Puesto que ASP.NET Identity y autenticación de cookies de OWIN son sistema basada en notificaciones, el marco de trabajo requiere que el desarrollador de la aplicación generar un [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) para el usuario. ClaimsIdentity tiene información sobre todas las notificaciones para el usuario, como los Roles que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase.
    > - Puede iniciar sesión el usuario mediante el uso de AuthenticationManager de OWIN y llamar al método `SignIn` y pasar la ClaimsIdentity como se indicó anteriormente. Este código se el usuario inicie sesión y generar también una cookie. Esta llamada es análoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilizado por el [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
2. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar**y, a continuación, **formulario Web Forms**. Nombre del formulario web Forms **inicio de sesión**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Reemplace el contenido de la *Login.aspx* archivo con el código siguiente:

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Reemplace el contenido de la *Login.aspx.cs* archivo por lo siguiente:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - El `Page_Load` ahora comprueba el estado de usuario actual y toma medidas según su `Context.User.Identity.IsAuthenticated` estado.
    >     **Para mostrar se registra en el nombre de usuario** : El marco de identidad de ASP.NET de Microsoft ha agregado los métodos de extensión en [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) que le permite obtener la `UserName` y `UserId` para el usuario inició sesión. Estos métodos de extensión se definen en el `Microsoft.AspNet.Identity.Core` ensamblado. Estos métodos de extensión son el reemplazo de [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Método SignIn: `This` reemplaza el anterior método `CreateUser_Click` un método en este ejemplo y ahora inicia sesión el usuario después de crear correctamente el usuario.   
    >   El marco de trabajo de OWIN de Microsoft ha agregado métodos de extensión en `System.Web.HttpContext` que le permite obtener una referencia a un `IOwinContext`. Estos métodos de extensión se definen en `Microsoft.Owin.Host.SystemWeb` ensamblado. El `OwinContext` clase expone un `IAuthenticationManager` propiedad que representa la funcionalidad de middleware de autenticación disponible en la solicitud actual. Puede iniciar sesión el usuario mediante el uso de la `AuthenticationManager` de OWIN y llamar al método `SignIn` y pasando el `ClaimsIdentity` como se indicó anteriormente. Dado que ASP.NET Identity y OWIN Cookie de autenticación basada en notificaciones de sistema, el marco de trabajo requiere que la aplicación para generar un `ClaimsIdentity` para el usuario. El `ClaimsIdentity` tiene información sobre todas las notificaciones del usuario, como los roles que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase, este código se el usuario inicie sesión y generar también una cookie. Esta llamada es análoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilizado por el [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
    > - `SignOut` método: Obtiene una referencia a la `AuthenticationManager` de OWIN y llama a `SignOut`. Esto es análogo a [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) método utilizado por el [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
5. Presione **Ctrl + F5** para compilar y ejecutar la aplicación web. Escriba un nuevo nombre de usuario y contraseña y, a continuación, seleccione **registrar**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Nota: En este momento, el nuevo usuario se crea y ha iniciado sesión.
6. Seleccione el **cerrar sesión** botón. Se le redirigirá al registro en el formulario.
7. Escriba un nombre de usuario no válido o la contraseña y seleccione el **iniciarla** botón. 
   El `UserManager.Find` método devolverá null y el mensaje de error: " *Nombre de usuario no válido o la contraseña* "se mostrará.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
