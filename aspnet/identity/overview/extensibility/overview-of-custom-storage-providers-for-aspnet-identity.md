---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: "Información general sobre proveedores de almacenamiento personalizado para identidades de ASP.NET | Documentos de Microsoft"
author: tfitzmac
description: "Identidad de ASP.NET es un sistema extensible que permite crear su propio proveedor de almacenamiento y conectarlo a la aplicación sin necesidad de volver a trabajar el fac..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: f43f0a2dd80e26ecff15e5742e18264ddb5b26aa
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Información general sobre proveedores de almacenamiento personalizado para identidades de ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Identidad de ASP.NET es un sistema extensible que permite crear su propio proveedor de almacenamiento y conectarlo a la aplicación sin necesidad de volver a trabajar la aplicación. En este tema se describe cómo crear un proveedor de almacenamiento personalizado de ASP.NET Identity. Abarca los conceptos más importantes para crear su propio proveedor de almacenamiento, pero no es un tutorial paso a paso de implementación de un proveedor de almacenamiento personalizado.
> 
> Para obtener un ejemplo de implementación de un proveedor de almacenamiento personalizado, consulte [implementar un proveedor de almacenamiento de información de identidad de ASP.NET MySQL personalizado](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> En este tema se ha actualizado para ASP.NET 2.0 de identidad.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Visual Studio 2013 Update 2
> - Identidad de ASP.NET 2


## <a name="introduction"></a>Introducción

De forma predeterminada, el sistema de identidades de ASP.NET almacena información de usuario en una base de datos de SQL Server y usa Entity Framework Code First para crear la base de datos. Para muchas aplicaciones, este enfoque funciona bien. Sin embargo, quizás prefiera usar un tipo de mecanismo de persistencia, como almacenamiento de tablas de Azure, o quizás ya tenga tablas de base de datos con una estructura muy distinta a la implementación predeterminada. En cualquier caso, puede escribir un proveedor personalizado para su mecanismo de almacenamiento y conecte dicho proveedor de la aplicación.

Identidad de ASP.NET se incluye de forma predeterminada en muchas de las plantillas de Visual Studio 2013. Puede obtener las actualizaciones a la identidad de ASP.NET a través de [paquete NuGet de Microsoft AspNet Identity EntityFramework](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

En este tema, se incluyen las siguientes secciones:

- [Comprender la arquitectura](#architecture)
- [Comprender los datos que se almacenarán](#data)
- [Crear la capa de acceso a datos](#dal)
- [Personalizar la clase de usuario](#user)
- [Personalizar el almacén del usuario](#userstore)
- [Personalizar el role (clase)](#role)
- [Personalizar el almacén de roles](#rolestore)
- [Volver a configurar la aplicación para que use el nuevo proveedor de almacenamiento](#reconfigure)
- [Otras implementaciones de proveedores de almacenamiento personalizado](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Comprender la arquitectura

Identidad de ASP.NET consta de las clases denominadas administradores y almacenes. Los administradores son las clases de alto nivel que un desarrollador de aplicaciones que se usa para realizar operaciones, como la creación de un usuario, en el sistema de identidades de ASP.NET. Los almacenes son las clases de nivel inferior que especifican cómo se conservan las entidades, como usuarios y roles. Los almacenes se acoplan estrechamente con el mecanismo de persistencia, pero se desacoplan administradores de almacenes de lo que significa que puede reemplazar el mecanismo de persistencia sin interrumpir toda la aplicación.

El siguiente diagrama muestra cómo interactúa la aplicación web con los administradores y almacenes de interactúan con la capa de acceso a datos.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Para crear un proveedor de almacenamiento personalizado de ASP.NET Identity, tendrá que crear el origen de datos, la capa de acceso a datos y las clases de almacén que interactúan con este nivel de acceso de datos. Para continuar usando el mismo administrador de API para realizar operaciones de datos en el usuario, pero ahora que los datos se guardan en un sistema de almacenamiento diferente.

No es necesario personalizar las clases de administrador porque al crear una nueva instancia de UserManager o RoleManager proporcionan el tipo de la clase de usuario y pase una instancia de la clase de almacenamiento como un argumento. Este enfoque permite conectar las clases personalizadas en la estructura existente. Verá cómo crear una instancia UserManager y RoleManager con las clases de almacén personalizado en la sección [volver a configurar la aplicación para que use el nuevo proveedor de almacenamiento](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Comprender los datos que se almacenarán

Para implementar un proveedor de almacenamiento personalizado, debe comprender los tipos de datos que se usan con la identidad de ASP.NET y decidir qué características son relevantes para la aplicación.

| Datos | Descripción |
| --- | --- |
| Usuarios | Usuarios registrados de su sitio web. Incluye el nombre de usuario y el Id. de usuario. Puede incluir una contraseña con hash si los usuarios inician sesión con credenciales que son específicas de su sitio (en lugar de con credenciales de un sitio externo como Facebook) y el sello de seguridad para indicar si algo ha cambiado en las credenciales de usuario. Puede también incluir dirección de correo electrónico, número de teléfono, si está habilitada la autenticación en dos fases, el número actual de error inicios de sesión, y si se ha bloqueado una cuenta. |
| Notificaciones de usuario | Un conjunto de instrucciones (o notificaciones) sobre el usuario que representan la identidad del usuario. Puede habilitar la expresión mayor de la identidad del usuario que se puede lograr a través de roles. |
| Inicios de sesión de usuario | Información sobre el proveedor de autenticación externo (por ejemplo, Facebook) que se usará cuando un usuario de inicio de sesión. |
| Roles | Grupos de autorización para el sitio. Incluye el nombre de identificador y el rol del rol (por ejemplo, "Admin" o "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Crear la capa de acceso a datos

En este tema se da por supuesto que está familiarizado con el mecanismo de persistencia que se va a usar y cómo crear entidades para dicho mecanismo. En este tema no proporciona detalles acerca de cómo crear los repositorios o clases de acceso a datos; en su lugar, proporciona algunas sugerencias acerca de las decisiones de diseño que se debe realizar cuando se trabaja con la identidad de ASP.NET.

Tiene mucha libertad al diseñar los repositorios para una personalizada proveedor del almacén. Basta con crear repositorios de características que piensa usar en la aplicación. Por ejemplo, si no usa roles en la aplicación, no es necesario crear almacenamiento para roles o los roles de usuario. La tecnología y la infraestructura existente pueden requerir una estructura que es muy diferente de la implementación predeterminada de ASP.NET Identity. En la capa de acceso a datos, proporcionar la lógica para trabajar con la estructura de los repositorios.

Para una implementación de MySQL de repositorios de datos de identidad de ASP.NET 2.0, consulte [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

En la capa de acceso a datos, proporcionar la lógica para guardar los datos de la identidad de ASP.NET en el origen de datos. La capa de acceso a datos para su proveedor de almacenamiento personalizado podría incluir las siguientes clases para almacenar la información de usuario y el rol.

| Clase | Descripción | Ejemplo |
| --- | --- | --- |
| Contexto | Encapsula la información para conectarse a su mecanismo de persistencia y ejecutar consultas. Esta clase es fundamental para la capa de acceso a datos. Las demás clases de datos requerirá una instancia de esta clase para realizar sus operaciones. También se inicializará las clases de almacén con una instancia de esta clase. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Almacenamiento de información de usuario | Almacena y recupera información de usuario (por ejemplo, el hash de nombre y la contraseña de usuario). | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Almacenamiento de rol | Almacena y recupera información de funciones (por ejemplo, el nombre de rol). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Almacenamiento de UserClaims | Almacena y recupera información de notificaciones de usuario (por ejemplo, el tipo de notificación y el valor). | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Almacenamiento de UserLogins | Almacena y recupera información de inicio de sesión de usuario (por ejemplo, un proveedor de autenticación externo). | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Almacenamiento de UserRole | Almacena y recupera los roles de un usuario se le asigna. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

De nuevo, solo debe implementar las clases que se desea utilizar en la aplicación.

En las clases de acceso a datos, proporcionar código para realizar operaciones de datos para su mecanismo de persistencia determinado. Por ejemplo, dentro de la implementación de MySQL, la clase UserTable contiene un método para insertar un nuevo registro en la tabla de base de datos de los usuarios. La variable denominada `_database` es una instancia de la clase MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Después de crear las clases de acceso de datos, debe crear las clases de almacén que llaman a los métodos específicos de la capa de acceso a datos.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Personalizar la clase de usuario

Al implementar su propio proveedor de almacenamiento, debe crear una clase de usuario que es equivalente a la [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) clase en el [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) espacio de nombres:

El siguiente diagrama muestra la clase IdentityUser que debe crear y la interfaz para implementar en esta clase.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

El [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) interfaz define las propiedades que UserManager intenta llamar al realizar operaciones solicitadas. La interfaz contiene dos propiedades: Id. y nombre de usuario. El [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) le permite especificar el tipo de la clave para el usuario a través de la interfaz genérica **TKey** parámetro. El tipo de la propiedad Id coincide con el valor del parámetro TKey.

El marco de trabajo de identidad también proporciona la [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) interfaz (sin el parámetro genérico) cuando desea utilizar un valor de cadena para la clave.

La clase IdentityUser implementa IUser y contiene las propiedades adicionales o constructores para los usuarios en el sitio web. En el ejemplo siguiente se muestra una clase IdentityUser que utiliza un entero para la clave. El campo Id está establecido en **int** para que coincida con el valor del parámetro genérico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Para una implementación completa, consulte [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Personalizar el almacén del usuario

También creará una clase UserStore que proporciona los métodos para todas las operaciones de datos en el usuario. Esta clase es equivalente a la [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) clase en el [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) espacio de nombres. En la clase UserStore, se implementa el [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) y cualquiera de las interfaces opcionales. Seleccione qué interfaces opcionales que se implementarán en función de la funcionalidad que se va a proporcionar en la aplicación.

La siguiente imagen muestra la clase userstore que debe crear y las interfaces correspondientes.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

La plantilla de proyecto predeterminada en Visual Studio contiene código que supone que muchas de las interfaces opcionales se han implementado en el almacén del usuario. Si usas la plantilla predeterminada con un almacén de usuario personalizada, debe implementar interfaces opcionales en el almacén de usuario o modificar el código de plantilla para ya no llamar a métodos en las interfaces que no ha implementado.

 En el ejemplo siguiente se muestra una clase de almacén de usuario sencilla. El **TUser** parámetro genérico toma el tipo de la clase de usuario que normalmente es la clase IdentityUser definida. El **TKey** parámetro genérico toma el tipo de la clave de usuario. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 En este ejemplo, se llama al constructor que toma un parámetro *base de datos* de tipo ExampleDatabase es sólo una ilustración de cómo pasar de la clase de acceso a datos. Por ejemplo, en la implementación de MySQL, este constructor toma un parámetro de tipo MySQLDatabase. 

Dentro de la clase UserStore, se utilizan las clases de acceso de datos que ha creado para realizar operaciones. Por ejemplo, en la implementación de MySQL, la clase UserStore tiene el método CreateAsync que usa una instancia de UserTable para insertar un nuevo registro. El **insertar** método en el **userTable** objeto es el mismo método que se muestra en la sección anterior. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfaces para implementar al personalizar el almacén de usuario

La siguiente imagen muestra más detalles sobre la funcionalidad definida en cada interfaz. Todas las interfaces opcionales heredan de IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
 El [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) interfaz es la única interfaz que debe implementar en el almacén de usuario. Define métodos para crear, actualizar, eliminar y recuperar los usuarios.
- **IUserClaimStore**  
 El [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) interfaz define los métodos debe implementar en el almacén de usuario para habilitar notificaciones de usuario. Contiene métodos o adición, eliminación y recuperación de notificaciones de usuario.
- **IUserLoginStore**  
 El [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) define los métodos debe implementar en el almacén de usuario para permitir a los proveedores de autenticación externo. Contiene métodos para agregar, quitar y recuperar los inicios de sesión de usuario y un método para recuperar un usuario basándose en la información de inicio de sesión.
- **IUserRoleStore**  
 El [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) interfaz define los métodos debe implementar en el almacén de usuario para asignar un usuario a un rol. Contiene métodos para agregar, quitar y recuperar los roles de usuario y un método para comprobar si un usuario está asignado a un rol.
- **IUserPasswordStore**  
 El [IUserPasswordStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) interfaz define los métodos que debe implementar en el almacén de usuario para almacenar contraseñas con algoritmo hash. Contiene métodos para obtener y establecer la contraseña con hash y un método que indica si el usuario ha establecido una contraseña.
- **IUserSecurityStampStore**  
 El [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) interfaz define los métodos que debe implementar en el almacén de usuario en una marca de seguridad que indica si ha cambiado la información de la cuenta del usuario . Esta marca se actualiza cuando un usuario cambia la contraseña, o agregue o quite inicios de sesión. Contiene métodos para obtener y establecer la marca de seguridad.
- **IUserTwoFactorStore**  
 El [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) interfaz define los métodos que debe implementar para implementar autenticación en dos fases. Contiene métodos para obtener y establecer si está habilitada la autenticación en dos fases para un usuario.
- **IUserPhoneNumberStore**  
 El [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) interfaz define los métodos que debe implementar para almacenar números de teléfono del usuario. Contiene métodos para obtener y establecer el número de teléfono y si se ha confirmado el número de teléfono.
- **IUserEmailStore**  
 El [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) interfaz define los métodos que debe implementar para almacenar direcciones de correo electrónico del usuario. Contiene métodos para obtener y establecer la dirección de correo electrónico y si se ha confirmado el correo electrónico.
- **IUserLockoutStore**  
 El [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) interfaz define los métodos que debe implementar para almacenar información acerca de los bloqueos de una cuenta. Contiene métodos para obtener el número actual de intentos de acceso erróneos, obtener y establecer si se puede bloquear la cuenta, obtener y establecer la fecha de finalización de bloqueo, incrementar el número de intentos erróneos y restablecer el número de intentos fallidos.
- **IQueryableUserStore**  
 El [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) interfaz define los miembros que se debe implementar para proporcionar un almacén de usuarios consultable. Contiene una propiedad que contiene los usuarios consultables.

 Implementar las interfaces que son necesarios en la aplicación; como el IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore y IUserSecurityStampStore interfaces tal y como se muestra a continuación. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Para una implementación completa (incluidas todas interfaces), consulte [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin y IdentityUserRole

El espacio de nombres Microsoft.AspNet.Identity.EntityFramework contiene implementaciones de la [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), y [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) clases. Si está usando estas características, puede crear sus propias versiones de estas clases y definir las propiedades de la aplicación. Sin embargo, a veces resulta más eficaz para no cargar estas entidades en memoria al realizar operaciones básicas (como agregar o quitar la notificación del usuario). En su lugar, las clases de almacenamiento back-end pueden ejecutar estas operaciones directamente en el origen de datos. Por ejemplo, el método UserStore.GetClaimsAsync() puede llamar a la userClaimTable.FindByUserId(user. Método de Id.) para ejecutar una consulta en que directamente de la tabla y devuelve una lista de notificaciones.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Personalizar el role (clase)

Al implementar su propio proveedor de almacenamiento, debe crear una clase de rol que es equivalente a la [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) clase en el [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) espacio de nombres:

El siguiente diagrama muestra la clase IdentityRole que debe crear y la interfaz para implementar en esta clase.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

El [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) interfaz define las propiedades que el RoleManager intenta llamar al realizar operaciones solicitadas. La interfaz contiene dos propiedades: Id y Name. El [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) le permite especificar el tipo de la clave para el rol a través de la interfaz genérica **TKey** parámetro. El tipo de la propiedad Id coincide con el valor del parámetro TKey.

El marco de trabajo de identidad también proporciona la [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) interfaz (sin el parámetro genérico) cuando desea utilizar un valor de cadena para la clave.

En el ejemplo siguiente se muestra una clase IdentityRole que utiliza un entero para la clave. El campo Id se establece en int para que coincida con el valor del parámetro genérico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Para una implementación completa, consulte [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Personalizar el almacén de roles

También creará una clase RoleStore que proporciona los métodos para todas las operaciones de datos en los roles. Esta clase es equivalente a la [RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) clase en el espacio de nombres Microsoft.ASP.NET.Identity.EntityFramework. En la clase RoleStore, se implementa el [IRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) y, opcionalmente, el [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) interfaz.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

En el ejemplo siguiente se muestra una clase de almacén de rol. El parámetro genérico TRole toma el tipo de la clase de función que normalmente es la clase IdentityRole definida. El parámetro genérico TKey toma el tipo de la clave de rol. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
 El [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) interfaz define los métodos que se implementan en la clase de almacén de rol. Contiene métodos para crear, actualizar, eliminar y recuperar roles.
- **RoleStore&lt;TRole&gt;**  
 Para personalizar RoleStore, cree una clase que implementa la interfaz IRoleStore. Solo tiene que implementar esta clase si desea utilizar las funciones del sistema. El constructor que toma un parámetro denominado *base de datos* de tipo ExampleDatabase es sólo una ilustración de cómo pasar de la clase de acceso a datos. Por ejemplo, en la implementación de MySQL, este constructor toma un parámetro de tipo MySQLDatabase.  
  
 Para una implementación completa, consulte [RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Volver a configurar la aplicación para que use el nuevo proveedor de almacenamiento

Ha implementado el nuevo proveedor de almacenamiento. Ahora, debe configurar la aplicación para usar este proveedor de almacenamiento. Si el proveedor de almacenamiento predeterminado se incluye en el proyecto, debe quitar el proveedor predeterminado y reemplazarlo con su proveedor.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Reemplace el proveedor de almacenamiento de forma predeterminada en el proyecto de MVC

1. En el **administrar paquetes de NuGet** ventana, desinstale el **Microsoft ASP.NET Identity EntityFramework** paquete. Puede encontrar este paquete buscando Identity.EntityFramework en los paquetes instalados.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png)Se le pedirá si también desea desinstalar Entity Framework. Si no se necesita en otras partes de la aplicación, puede desinstalarlo.
2. En el archivo IdentityModels.cs en la carpeta Models, elimine o marque como comentario el **ApplicationUser** y **ApplicationDbContext** clases. En una aplicación MVC, puede eliminar todo el archivo IdentityModels.cs. En una aplicación de formularios Web Forms, eliminar las dos clases, pero debe asegurarse de que mantener la clase auxiliar que también se encuentra en el archivo IdentityModels.cs.
3. Si su proveedor de almacenamiento reside en un proyecto independiente, agregue una referencia a él en la aplicación web.
4. Reemplace todas las referencias a `using Microsoft.AspNet.Identity.EntityFramework;` con el uso de una instrucción para el espacio de nombres del proveedor de almacenamiento.
5. En el **Startup.Auth.cs** clase, cambie el **ConfigureAuth** método se debe utilizar una única instancia del contexto adecuado. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. En la aplicación\_carpeta de inicio, abra **IdentityConfig.cs**. En la clase ApplicationUserManager, cambie la **Create** método devuelva un administrador de usuario que utiliza el almacén de usuario personalizada. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Reemplace todas las referencias a **ApplicationUser** con **IdentityUser**.
8. El proyecto predeterminado incluye a algunos miembros de clase de usuario que no estén definidos en la interfaz IUser; Por ejemplo, correo electrónico, PasswordHash y GenerateUserIdentityAsync. Si su clase de usuario no tiene estos miembros, debe implementarlos o cambiar el código que utiliza a estos miembros.
9. Si ha creado todas las instancias de RoleManager, cambiar el código para usar la nueva clase RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. El proyecto predeterminado está diseñado para una clase de usuario que tiene un valor de cadena para la clave. Si su clase de usuario tiene un tipo diferente de la clave (por ejemplo, un número entero), debe cambiar el proyecto para que funcione con su tipo. Vea [cambiar la clave principal para los usuarios en ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Si es necesario, agregue la cadena de conexión en el archivo Web.config.

<a id="other"></a>
## <a name="other-resources"></a>Otros recursos

- Blog: [implementar Identity de ASP.NET](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Código del tutorial y GIT: [Simple.Data proveedor de identidades de Asp.Net](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Tutorial:[cómo configurar las cuentas de identidad básicas y dirijan en una base de datos externo](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Por [ @xivSolutions ](https://twitter.com/xivSolutions).
- Tutorial[: implementar un proveedor de almacenamiento de la identidad de ASP.NET de MySQL personalizado](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entidades de CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) por [SoftFluent](http://www.softfluent.com/)
- [Almacenamiento de tabla de Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) por James Randall.
- Almacenamiento de tabla de Azure: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) por [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant por Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Bús elástico[h: identidad elástico](https://github.com/bmbsqd/elastic-identity) por AB de Bombsquad.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) por Jonathan Sheely Jonathan Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) por Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) por [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) por [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Plantillas T4 EF de generar código para un almacén de usuario "base de datos en primer lugar": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
