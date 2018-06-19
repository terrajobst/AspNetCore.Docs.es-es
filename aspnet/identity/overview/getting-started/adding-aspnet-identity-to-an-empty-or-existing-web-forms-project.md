---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Agregar la identidad de ASP.NET a un Web existente o vacía Forms proyecto | Documentos de Microsoft
author: raquelsa
description: Este tutorial muestra cómo agregar identidades de ASP.NET (el nuevo sistema de pertenencia de ASP.NET) a una aplicación ASP.NET. Cuando se crea un nuevo formularios Web Forms o MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8961e596f0d6cc4810e2439be1ec2915bddb8c78
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874660"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Proyecto de forma de agregar identidades de ASP.NET a un Web vacío o existente
====================
por [Raquel Soares Alemania Almeida](https://github.com/raquelsa)

> Este tutorial muestra cómo agregar [ASP.NET Identity](introduction-to-aspnet-identity.md) (el nuevo sistema de pertenencia de ASP.NET) a una aplicación ASP.NET.
> 
> Cuando crea un nuevo proyecto de formularios Web Forms o MVC en Visual Studio 2013 RTM con cuentas individuales, Visual Studio instalará todos los paquetes necesarios y agregar todas las clases necesarias para usted. Este tutorial muestra los pasos para agregar compatibilidad de ASP.NET Identity para el proyecto de formularios Web Forms existente o un nuevo proyecto vacío. Se resumirán todos los paquetes de NuGet que se debe instalar y clases que se debe agregar. Pasará a través de formularios Web Forms de ejemplo para registrar los nuevos usuarios y el registro mientras resalta todas las API de punto de entrada principal para la autenticación y administración de usuarios. Este ejemplo usará la implementación predeterminada de ASP.NET Identity para el almacenamiento de datos SQL que se basa en Entity Framework. Este tutorial, usaremos LocalDB para la base de datos SQL.
> 
> Este tutorial se escribió Raquel Soares De Almeida y Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Introducción a la identidad de ASP.NET

1. Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Haga clic en **nuevo proyecto** desde el principio página, o puede utilizar el menú y seleccione **archivo**y, a continuación, **nuevo proyecto**.
3. Seleccione **Visual C# i** n el panel izquierdo, a continuación, **Web** y, a continuación, seleccione **aplicación Web ASP.NET**. Denomine el proyecto "WebFormsIdentity" y, a continuación, haga clic en **Aceptar**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **vacía** plantilla.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Observe el **Cambiar autenticación** botón está deshabilitado y no hay compatibilidad con la autenticación se proporciona en esta plantilla. Las plantillas de formularios Web Forms, MVC y API Web le permiten seleccionar el método de autenticación. Para obtener más información, consulte [información general de la autenticación](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Agregar paquetes de identidad a la aplicación

En el Explorador de soluciones, haga clic en el proyecto y seleccione **administrar paquetes de NuGet**. En el cuadro de diálogo de cuadro de texto de búsqueda, escriba "*Identity.E*". Haga clic en instalar para este paquete.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Tenga en cuenta que este paquete instalará los paquetes de dependencia: Entity Framework y Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Adición de formularios Web Forms para registrar los usuarios

1. En **el Explorador de soluciones**, haga clic en el proyecto y haga clic en **agregar**y, a continuación, **formulario Web**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. En el **especificar nombre para el elemento** cuadro de diálogo, el nombre del nuevo formulario web **registrar**y, a continuación, haga clic en **Aceptar**
3. Reemplace el marcado en generado *Register.aspx* archivo con el código siguiente. Los cambios de código aparecen resaltados.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Esto es simplemente una versión simplificada de la *Register.aspx* archivo que se crea cuando se crea un nuevo proyecto de formularios Web Forms de ASP.NET. El marcado anterior agrega campos de formulario y un botón para registrar un nuevo usuario.
4. Abra la *Register.aspx.cs* archivo y reemplace el contenido del archivo con el código siguiente:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. El código anterior es una versión simplificada de la *Register.aspx.cs* archivo que se crea cuando se crea un nuevo proyecto de formularios Web Forms de ASP.NET.
    > 2. El *IdentityUser* clase es la implementación de EntityFramework predeterminada de la *IUser* interfaz. *IUser* interfaz es la interfaz básica para un usuario en ASP.NET Identity Core.
    > 3. El *UserStore* clase es la implementación de EntityFramework predeterminada de un almacén de usuario. Esta clase implementa interfaces mínimas de ASP.NET Identity Core: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* y *IUserRoleStore* .
    > 4. El *UserManager* clase expone usuario relacionados con las API que guardarán automáticamente los cambios realizados en el *UserStore*.
    > 5. El *IdentityResult* clase representa el resultado de una operación de identidad.
5. En **el Explorador de soluciones**, haga clic en el proyecto y haga clic en **agregar**, **Agregar carpeta ASP.NET** y, a continuación, **aplicación\_datos**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Abra la *Web.config* de archivos y agregue una entrada de cadena de conexión para la base de datos que se usará para almacenar información del usuario. Se creará la base de datos en tiempo de ejecución por Entity Framework para las entidades de identidad. La cadena de conexión es similar a la que se crea automáticamente al crear un nuevo proyecto de formularios Web Forms. El código resaltado muestra el marcado que debe agregar:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Para Visual Studio 2015 o versiones posteriores, reemplace `(localdb)\v11.0` con `(localdb)\MSSQLLocalDB` en la cadena de conexión.
    
7. Haga clic en el archivo *Register.aspx* en el proyecto y seleccione **establecer como página de inicio**. Presione Ctrl + F5 para compilar y ejecutar la aplicación web. Escriba un nuevo nombre de usuario y una contraseña y, a continuación, haga clic en **registrar**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity tiene compatibilidad para la validación y en este ejemplo puede comprobar el comportamiento predeterminado del usuario y contraseña validadores que proceden del paquete principal de identidad. El validador de manera predeterminada para el usuario (`UserValidator`) tiene una propiedad `AllowOnlyAlphanumericUserNames` que tiene el valor predeterminado establecido en `true`. El validador predeterminado para la contraseña (`MinimumLengthValidator`) garantiza que la contraseña tenga al menos 6 caracteres. Validadores de estas propiedades se encuentran en `UserManager` que se puede invalidar si desea tener validación personalizada,

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Comprobar la base de datos de identidad de LocalDb y las tablas generadas por Entity Framework

1. En el **vista** menú, haga clic en **Explorador de servidores**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Expanda **DefaultConnection (WebFormsIdentity)**, expanda **tablas**, haga clic en **AspNetUsers** y haga clic en **mostrar datos de tabla**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Configuración de la aplicación para la autenticación de OWIN

En este momento sólo hemos agregado compatibilidad para la creación de usuarios. Ahora, vamos a mostrar cómo podemos agregar autenticación para iniciar sesión un usuario. Identidad de ASP.NET usa middleware de autenticación de Microsoft OWIN para la autenticación de formularios. La autenticación con cookies OWIN es una cookie y reclama el mecanismo de autenticación basada en que se puede usar cualquier marco de trabajo hospedado en [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) o IIS. Con este modelo, los mismos paquetes de autenticación pueden utilizarse en varios entornos, incluidos ASP.NET MVC y formularios Web Forms. Para obtener más información sobre el proyecto Katana y cómo ejecutar middleware en un host independiente, vea [introducción con el proyecto de Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Instalación de paquetes de autenticación en la aplicación

1. En el Explorador de soluciones, haga clic en el proyecto y seleccione **administrar paquetes de NuGet**. En el cuadro de diálogo de cuadro de texto de búsqueda, escriba "*Identity.Owin*". Haga clic en instalar para este paquete.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Busque el paquete ***Microsoft.Owin.Host.SystemWeb*** e instalarlo.   

    > [!NOTE]
    > El **Microsoft.Aspnet.Identity.Owin** paquete contiene un conjunto de clases de extensión OWIN para administrar y configurar el middleware de autenticación OWIN a ser consumidos por los paquetes de ASP.NET Identity Core.  
    > El **Microsoft.Owin.Host.SystemWeb** paquete contiene un servidor OWIN que permite que aplicaciones basadas en OWIN para ejecutar en IIS mediante la canalización de solicitudes ASP.NET. Para obtener más información, consulte [Middleware de OWIN en IIS integrado canalización](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Agregar clases de configuración de autenticación y de inicio de OWIN

1. En **el Explorador de soluciones**, haga clic en el proyecto, haga clic en **agregar**y, a continuación, **Agregar nuevo elemento**. En el cuadro de diálogo de cuadro de texto de búsqueda, escriba "*owin*". La clase el nombre "*inicio*" y haga clic en **agregar**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. En el archivo Startup.cs, agregue el código resaltado que se muestra a continuación para configurar la autenticación de cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Esta clase contiene la `OwinStartup` atributo para especificar la clase de inicio OWIN. Todas las aplicaciones OWIN tiene una clase de inicio donde se especifican los componentes de la canalización de la aplicación. Vea [detección de clase de inicio de OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) para obtener más información sobre este modelo.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Adición de formularios Web Forms para registrar e iniciar sesión a los usuarios

1. Abra la *Register.cs* de archivos y agregue el código siguiente a la que se registrará en el usuario al registro es satisfactorio. A continuación, se resaltan los cambios.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Puesto que ASP.NET Identity y autenticación con cookies OWIN son sistema basada en notificaciones, el marco de trabajo requiere que el desarrollador de aplicaciones generar un [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) para el usuario. Valor de ClaimsIdentity incluye información sobre todas las notificaciones para el usuario, como los Roles que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase.
    > - Puede iniciar sesión en el usuario mediante el uso de la clase AuthenticationManager de OWIN y llamar al método `SignIn` y pasar el valor de ClaimsIdentity como se indicó anteriormente. Este código se inicie sesión en el usuario y generar también un cookie. Esta llamada es análoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilizado por el [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
2. En **el Explorador de soluciones**, haga clic en el proyecto, haga clic **agregar**y, a continuación, **formulario Web**. Un nombre al formulario web **inicio de sesión**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Reemplace el contenido de la *Login.aspx* archivo con el código siguiente:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Reemplace el contenido de la *Login.aspx.cs* archivo con lo siguiente:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - El `Page_Load` ahora comprueba el estado de usuario actual y toma medidas en función de su `Context.User.Identity.IsAuthenticated` estado.  
    >     **Mostrar Logged en nombre de usuario** : The Microsoft ASP.NET Identity Framework incorpora métodos de extensión en [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) que le permite obtener la `UserName` y `UserId` para el Usuario ha iniciado sesión. Estos métodos de extensión se definen en la `Microsoft.AspNet.Identity.Core` ensamblado. Estos métodos de extensión son el sustituto de [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Método de inicio de sesión:   
    >     `This` método reemplaza la anterior `CreateUser_Click` método en este ejemplo y ahora los signos de usuario después de crear correctamente el usuario.   
    >  El marco de trabajo de Microsoft OWIN incorpora métodos de extensión en `System.Web.HttpContext` que le permite obtener una referencia a un `IOwinContext`. Estos métodos de extensión se definen en `Microsoft.Owin.Host.SystemWeb` ensamblado. El `OwinContext` clase expone un `IAuthenticationManager` propiedad que representa la funcionalidad de middleware de autenticación disponible en la solicitud actual.  
    >  Puede iniciar sesión en el usuario mediante el uso de la `AuthenticationManager` de OWIN y llamar al método `SignIn` y pasando el `ClaimsIdentity` como se indicó anteriormente.   
    >  Dado que ASP.NET Identity y autenticación con cookies OWIN son sistema basado en notificaciones, el marco de trabajo requiere la aplicación para generar un `ClaimsIdentity` para el usuario.   
    >  El `ClaimsIdentity` tiene información sobre todas las notificaciones para el usuario, como los roles que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase  
    >  Este código se inicie sesión en el usuario y generar también un cookie. Esta llamada es análoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilizado por el [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
    > - `SignOut` método:   
    >  Obtiene una referencia a la `AuthenticationManager` OWIN y llama `SignOut`. Esto es análogo a [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) método utilizado por el [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
5. Presione **Ctrl + F5** para compilar y ejecutar la aplicación web. Escriba un nuevo nombre de usuario y una contraseña y, a continuación, haga clic en **registrar**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Nota: en este momento, el nuevo usuario se crea y registra en.
6. Haga clic en **cerrar sesión** botón. Se le redirigirá en el registro en el formulario.
7. Escriba un nombre de usuario no válido o su contraseña y haga clic **sesión** botón.   
   El `UserManager.Find` método devolverá null y el mensaje de error: " *nombre de usuario no válido o la contraseña* " se mostrará.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
