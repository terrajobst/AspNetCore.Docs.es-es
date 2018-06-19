---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Pertenencia | Documentos de Microsoft
author: microsoft
description: Pertenencia a ASP.NET se basa en el éxito del modelo de autenticación de formularios de ASP.NET 1.x. Autenticación de formularios ASP.NET proporciona una manera cómoda ensam...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 1a5a495845b60f9aac51c9776311af67f5dc8767
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885585"
---
<a name="membership"></a>Pertenencia
====================
por [Microsoft](https://github.com/microsoft)

> Pertenencia a ASP.NET se basa en el éxito del modelo de autenticación de formularios de ASP.NET 1.x. Autenticación de formularios ASP.NET proporciona una manera cómoda para incorporar un formulario de inicio de sesión en la aplicación ASP.NET y validar a los usuarios con una base de datos u otro almacén de datos.


Pertenencia a ASP.NET se basa en el éxito del modelo de autenticación de formularios de ASP.NET 1.x. Autenticación de formularios ASP.NET proporciona una manera cómoda para incorporar un formulario de inicio de sesión en la aplicación ASP.NET y validar a los usuarios con una base de datos u otro almacén de datos. Los miembros de la clase FormsAuthentication le permite administrar las cookies para la autenticación, busque un inicio de sesión válido, inicie sesión un usuario etcetera. Sin embargo, la implementación de autenticación de formularios en una aplicación ASP.NET 1.x puede requerir una cantidad considerable de código.

Pertenencia en ASP.NET 2.0 es un gran avance respecto al uso de autenticación de formularios por sí sola. (Pertenencia es más eficaz cuando se acopla con autenticación de formularios, pero no es un requisito usando la autenticación de formularios). Como verá pronto, puede utilizar pertenencia a ASP.NET y los controles de inicio de sesión en ASP.NET 2.0 para implementar un sistema de pertenencia eficaz sin escribir mucho código en absoluto.

## <a name="implementing-membership-in-aspnet-20"></a>Implementación de pertenencia en ASP.NET 2.0

Pertenencia se implementa mediante cuatro pasos. Tenga en cuenta que hay muchos pasos secundarios que están implicados, así como la configuración opcional que puede implementarse también. Estos pasos están pensados para ilustrar el panorama general de configuración de pertenencia.

1. Cree la base de datos de pertenencia (si SQL Server se utiliza como almacén de la suscripción.)
2. Especifique las opciones de pertenencia en los archivos de configuración de aplicaciones. (Pertenencia está habilitada de forma predeterminada).
3. Determinar el tipo de pertenencia del almacén que desea usar. Las opciones son: 

    - Microsoft SQL Server (versión 7.0 o posterior)
    - Almacén de Active Directory
    - Proveedor de pertenencia personalizado
4. Configure la aplicación para la autenticación de formularios de ASP.NET. Una vez más, pertenencia está diseñada para aprovechar las ventajas de la autenticación de formularios, pero no es un requisito usando la autenticación de formularios.
5. Definir las cuentas de usuario para la suscripción y configurar roles si lo desea.

## <a name="creating-the-membership-database"></a>Crear la base de datos de pertenencia

Si utiliza SQL Server 7.0 o posterior como almacén de pertenencia, puede usar aspnet\_utilidad regsql (disponible más fácilmente desde el Visual Studio .NET 2005 Command Prompt) para configurar la base de datos. Aspnet\_regsql utilidad puede utilizarse como una herramienta de línea de comandos o mediante un asistente de interfaz gráfica de usuario. El método del asistente es la manera más fácil de configurar la base de datos. Para tener acceso al asistente, simplemente ejecute el siguiente comando:

`aspnet_regsql W`

Una vez que ejecute este comando, aparecerá con el Asistente para la instalación de ASP.NET SQL Server tal y como se muestra a continuación.


![](membership/_static/image1.jpg)

**Figura 1**


El Asistente para la instalación de ASP.NET SQL Server, se crea el sitio Web en la instancia que se especifica en el asistente. Sin embargo, ASP.NET usará la cadena de conexión en el archivo machine.config para conectarse a la base de datos. De forma predeterminada, esta cadena de conexión señala a una instancia de SQL Server 2005, por ello, si se utiliza una instancia de SQL Server 2000 o SQL Server 7.0, debe modificar la cadena de conexión en el archivo machine.config. Esa cadena de conexión se puede encontrar aquí:

[!code-xml[Main](membership/samples/sample1.xml)]

Por desgracia, si no modifica la cadena de conexión, ASP.NET, no tendrá un error descriptivo. Simplemente, continuarán se quejan de que dice que no ha creado la base de datos. En el caso anterior, modifiqué la cadena de conexión para que señale a la instancia local de SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Especificar la configuración y agregar usuarios y Roles

El paso siguiente para configurar la pertenencia es agregar la información necesaria en el archivo web.config de la aplicación. En ASP.NET 1.x, modificar el archivo web.config en ocasiones resultaba difícil debido a la utilización de lowerCamelCase y la falta de Intellisense. Visual Studio .NET 2005 hace mucho más fácil con Intellisense para archivos de configuración de la tarea, pero ASP.NET 2.0 va un paso más allá al proporcionar una interfaz Web para editar archivos de configuración.

Puede iniciar la interfaz Web haciendo clic en el botón Configuración de ASP.NET en la barra de herramientas del explorador de soluciones tal y como se muestra a continuación. También puede iniciar la interfaz Web a través de elementos emergentes que se muestran cuando se insertan los controles de inicio de sesión.


![](membership/_static/image2.jpg)

**Figura 2**


Esto inicia la herramienta de administración de sitios Web de ASP.NET se muestra a continuación. La administración de sitios Web de ASP.NET es una interfaz de cuatro-ficha que facilita el proceso administrar la configuración de la aplicación. Están disponibles las siguientes pestañas:

- **Página principal**
- **Seguridad** configurar usuarios, roles y acceso.
- **Aplicación** configurar la aplicación.
- **Proveedor** configurar y probar el proveedor de pertenencia de las aplicaciones.

La herramienta de administración de sitios Web le permite fácilmente crear nuevos usuarios, crear nuevos roles y para administrar usuarios y roles. Esta capacidad no está disponible en la interfaz de Windows. La interfaz de Windows le permite definir fácilmente la configuración de autorización y agregar, eliminar y administrar los proveedores de funciones que no están en la herramienta de administración de sitios Web.

Para iniciar la interfaz de Windows, abra el complemento Servicios de Internet Information Server, haga doble clic en la aplicación y elija Propiedades. Haga clic en la ficha ASP.NET y, a continuación, haga clic en el botón de edición de la configuración. (La aplicación debe estar ejecutándose en ASP.NET 2.0 para el botón Editar configuración esté habilitado. Puede configurar la versión de ASP.NET en el cuadro de diálogo ASP.NET también). Tal y como se muestra a continuación, se muestra el cuadro de diálogo de opciones de configuración de ASP.NET.


![](membership/_static/image3.jpg)

**Figura 3**


En la ficha General, se enumeran las cadenas de conexión y configuración de la aplicación. Cualquier configuración en cursiva se define en un archivo de configuración principal (el archivo machine.config o un archivo web.config en un nivel superior) y configuraciones no está en cursiva son el archivo de configuración de aplicaciones. Si se agrega una configuración, quitar o editar en el nivel de aplicación, ASP.NET se agregar, quitar o modificar la configuración en el archivo web.config de niveles de la aplicación en lugar de quitar la configuración del archivo de configuración desde el que se ha heredado.

A continuación se muestra la pestaña autenticación. Esto es donde va a configurar la configuración de pertenencia. Formularios de configuración de autenticación, los proveedores de pertenencia, y los proveedores de funciones se pueden configurar aquí.


![](membership/_static/image4.jpg)

**Figura 4**


## <a name="implementing-membership-in-your-application"></a>Implementación de pertenencia en la aplicación

La manera más fácil de implementar la pertenencia de ASP.NET 2.0 en la aplicación es usar los controles de inicio de sesión proporcionados. Este método le permite implementar los conceptos básicos de pertenencia de ASP.NET 2.0 sin escribir ningún código en absoluto.

Los siguientes controles de inicio de sesión están disponibles en ASP.NET 2.0:

## <a name="login-control"></a>Control de inicio de sesión

El control de inicio de sesión proporciona una interfaz para que alguien inicie sesión en el sistema de pertenencia. Proporciona un nombre de usuario y contraseña textboxt y un botón de inicio de sesión. Muchas otras características comunes, como un vínculo para registrarse para las personas que aún no lo ha hecho por lo tanto, una casilla que permite al usuario iniciar sesión automáticamente en visitas posteriores, un vínculo para un recordatorio de contraseña, etcetera. Todas las características del control de inicio de sesión son personalizables a través de las propiedades del control.

En ASP.NET 1.x, los programadores tenían que escribir una cantidad considerable de código para realizar una búsqueda con autenticación de formularios. Con la pertenencia de ASP.NET 2.0, puede validar a los usuarios sin escribir ningún código en absoluto. ASP.NET realizará automáticamente la búsqueda del usuario automáticamente. (Si está utilizando el control de inicio de sesión sin usar la pertenencia a ASP.NET, puede usar el **OnAuthenticate** método para validar al usuario.)

## <a name="loginview-control"></a>Control LoginView

El control LoginView es un control con plantilla que proporciona dos plantillas de forma predeterminada. AnonymousTemplate y LoggedInTemplate. La plantilla que se muestra está determinada por si el usuario ha iniciado en el sistema de pertenencia. Este control normalmente se utiliza para mostrar un control de inicio de sesión cuando un usuario no ha iniciado aún y un control LoginStatus y/o otros controles de inicio de sesión cuando el usuario ha iniciado sesión. Si está utilizando la administración de funciones en la aplicación ASP.NET, el control LoginView puede mostrar una plantilla específica, según el rol de los usuarios. (Más en ASP.NET administración de roles se explicará más adelante.)

## <a name="passwordrecovery-control"></a>Control PasswordRecovery

El control PasswordRecovery permite a los usuarios recibirán un correo electrónico con su contraseña actual o restablecer su contraseña. Texto no cifrado y contraseñas cifradas se pueden recuperar y por correo electrónico a los usuarios. Si se aplica un algoritmo hash de la contraseña, no se puede recuperar. En su lugar, el usuario será necesario para realizar un restablecimiento de contraseña.

## <a name="loginstatus-control"></a>Control LoginStatus

El control LoginStatus se utiliza para mostrar un indicador de inicio de sesión a los usuarios que no se ha iniciado sesión y un indicador de cierre de sesión a los usuarios que han iniciado sesión en. La propiedad Request.IsAuthenticated se usa para determinar qué indicador para mostrar. El indicador muestra el control LoginStatus puede ser texto (implementado a través de la **LoginText** y **LogoutText** propiedades) o imágenes (implementado a través de la **LoginImageUrl**y **LogoutImageUrl** propiedades.)

Cuando un usuario cierra la sesión a través del control LoginStatus, que se redirige a la dirección URL especificada por el **LogoutPageUrl** propiedad. Si no se establece esta propiedad, se actualiza la página actual. Puesto que el sitio es probable que está protegido por la autenticación de formularios, la actualización de la página actual redirigirá al usuario a la página de inicio de sesión para el sitio.

## <a name="loginname-control"></a>Control LoginName

El control LoginName muestra el nombre de usuario del usuario que actualmente ha iniciado en el sitio.

## <a name="createuserwizard-control"></a>Control CreateUserWizard

El control CreateUserWizard proporciona a los usuarios una manera cómoda de registrarse para el sistema de pertenencia. Puede agregar pasos (se implementa como una colección de WizardSteps) a través de la interfaz que se muestra a continuación.


![](membership/_static/image5.jpg)

**Figura 5**


CreateUserWizard es un control con plantilla que se deriva de la clase de asistente y proporciona las siguientes plantillas:

- **HeaderTemplate** esta plantilla controla el aspecto del encabezado del asistente.
- **SidebarTemplate** esta plantilla controla el aspecto de la barra lateral del asistente.
- **StartNavigationTemplate** esta plantilla controles son la apariencia de la navegación del asistente en el paso de inicio.
- **StepNavigationTemplate** esta plantilla controla el aspecto del área de navegación cuando no está en el paso de inicio o fin.
- **FinishNavigationTemplate** esta plantilla controla el aspecto del área de navegación en el paso Finalizar.

Además, para cada paso que agregue al asistente, ASP.NET creará una plantilla personalizada que contiene una plantilla ContentTemplate y un CustomNavigationTemplate para ese paso. Para obtener detalles completos acerca de cómo personalizar la CreateUserWizard, consulte la documentación de VS.NET 2005:

## <a name="changepassword-control"></a>Control ChangePassword

El control ChangePassword permite a los usuarios cambiar su contraseña. Si la propiedad DisplayUserName es true (es false de forma predeterminada), el usuario puede cambiar su contraseña cuando no se ha iniciado. Si el usuario *es* ya inició sesión y la propiedad DisplayUserName es true, el usuario podrá cambiar la contraseña de otro usuario que no se registra en la provisión de que conocen el identificador de usuario de ese usuario.

Tenga en cuenta que si desea que los usuarios puedan cambiar las contraseñas sin tener que iniciar sesión, debe asegurarse de que la página en el que se muestra el control ChangePassword permite el acceso anónimo. Obviamente, los usuarios tendrán que proporcionar su contraseña antigua para cambiar su contraseña.

## <a name="role-management"></a>Administración de roles

Administración de funciones permite asignar a usuarios a un rol determinado y, a continuación, restringir el acceso a determinados archivos o carpetas en función de ese rol. Administración de funciones también proporciona una API para que mediante programación puede determinar la función someones o determinar todos los usuarios de un rol determinado y responder según corresponda.

Administración de roles no es un requisito de pertenencia a ASP.NET, ni es la pertenencia a un requisito para poder usar Administración de roles. Sin embargo, los dos complementan entre sí correctamente y es probable que los desarrolladores utilicen ellos junto con entre sí.

Para habilitar la administración de roles en su aplicación, realice el siguiente cambio en el archivo web.config:

[!code-xml[Main](membership/samples/sample2.xml)]

Cuando el **cacheRolesInCookie** atributo está establecido en true, ASP.NET almacena en caché una pertenencia a roles de usuarios en una cookie en el cliente. Esto permite realizar búsquedas de rol que se produzca sin llamadas en el RoleProvider. Cuando se utiliza este atributo, los desarrolladores pueden para asegurarse de que el **cookieProtection** atributo está establecido en todos. (Este es el valor predeterminado). Esto garantiza que los datos de las cookies se cifran y ayuda a garantizar que aún no se ha modificado el contenido de las cookies. Roles se pueden agregar utilizando la herramienta de administración de sitios Web. Permite definir los roles, configurar el acceso a las partes del sitio en función de esos roles y asignar a usuarios a roles fácilmente.


![](membership/_static/image6.jpg)

**Figura 6**


Como se indicó anteriormente, pueden agregarse nuevos roles basta con escribir el nombre de la función y, a continuación, haciendo clic en Agregar rol. Los roles existentes pueden ser administrados o eliminar haciendo clic en el vínculo apropiado en la lista de roles existentes.

Cuando administre un rol, puede agregar o quitar usuarios, tal y como se muestra a continuación.


![](membership/_static/image7.jpg)

**Figura 7**


Activando la casilla de verificación de la función del usuario, puede agregar fácilmente un usuario a un rol específico. ASP.NET se actualizará automáticamente la base de datos de pertenencia con las entradas correspondientes. También deberá configurar reglas de acceso para la aplicación. Los programadores de ASP.NET 1.x están familiarizados con hacerlo a través de la &lt;autorización&gt; elemento en el archivo web.config y esa opción aún está disponible en ASP.NET 2.0. Sin embargo, su más fáciles de administrar el acceso a las reglas con el sitio Web herramienta Administración tal como se muestra a continuación.


![](membership/_static/image8.jpg)

**Figura 8**


En este caso, se resalta la carpeta de administración (su difícil ver porque la herramienta resalta en color gris claro) y se ha concedido acceso a la función Administradores. Se deniegan todos los demás usuarios. Puede hacer clic en el icono principal para seleccionar una regla y, a continuación, utilice los botones Subir y Bajar para organizar las reglas. Al igual que con ASP.NET &lt;autorización&gt; elemento, las reglas se procesan en el orden en que aparecen. En otras palabras, si se invierte el orden de reglas en la captura anterior, nadie tendría acceso a la carpeta de administración porque la primera regla que ASP.NET encontraría sería la regla que deniega todos los usuarios a la carpeta.

ASP.NET 2.0 agrega un archivo web.config en la carpeta para el que está especificando una regla de acceso. Las reglas de acceso pueden modificarse mediante el archivo de configuración o mediante la herramienta de administración de sitios Web. En otras palabras, la herramienta de administración de sitios Web es simplemente una interfaz a través del cual se puede editar el archivo de configuración en un entorno fácil de usar.

## <a name="using-roles-in-code"></a>Usar funciones en el código

La API de administración de roles no ha cambiado desde la versión 1.x. El **IsInRole** método se utiliza para determinar si un usuario pertenece a un rol determinado.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET también crea una instancia de RolePrincipal como miembro del contexto actual. El objeto RolePrincipal puede utilizarse para obtener todos los roles a los que pertenece el usuario como se indica a continuación:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Usar RoleGroups con el Control LoginView

Ahora que tiene una descripción de la administración de roles y pertenencia, veamos brevemente cómo el control LoginView aprovecha las ventajas de esta capacidad en ASP.NET 2.0. Como se explicó anteriormente, el control LoginView es un control con plantilla que contiene dos plantillas de forma predeterminada. AnonymousTemplate y LoggedInTemplate. Dentro de las tareas LoginView diálogo es un vínculo (se muestra a continuación) que permite Editar RoleGroups.


![](membership/_static/image9.jpg)

**Figura 9**


Cada objeto RoleGroup contiene una matriz de cadenas que define qué roles que se aplica RoleGroup. Para agregar un nuevo RoleGroup al control LoginView, haga clic en el vínculo Editar RoleGroups. En la imagen anterior, puede ver que he agregado un nuevo RoleGroup para administradores. Si selecciona ese RoleGroup (RoleGroup[0]) en la lista desplegable de vistas, puedo configurar una plantilla que solo se mostrará a los miembros del rol de administradores. En la siguiente imagen, he agregado un nuevo RoleGroup que se aplica a los miembros del rol Sales y la función de distribución. Esto agrega un segundo RoleGroup a la lista desplegable de vistas en el cuadro de diálogo de tareas de LoginView y todo lo agregará a dicha plantilla se verá por cualquier usuario de la distribución o de ventas rol.


![](membership/_static/image10.jpg)

**Figura 10**


## <a name="overriding-the-existing-membership-provider"></a>Reemplazar el proveedor de pertenencia existente

Hay un par de maneras que puede ampliar la funcionalidad de pertenencia a ASP.NET. En primer lugar, puede cambiar la funcionalidad existente de la clase SqlMembershipProvider obviamente herede de él y reemplazando los métodos. Por ejemplo, si desea implementar su propia funcionalidad cuando se crean los usuarios, puede crear su propia clase que hereda de SqlMembershipProvider como sigue:

[!code-csharp[Main](membership/samples/sample5.cs)]

Si, por otro lado, desea crear su propio proveedor (para almacenar la información de pertenencia en una base de datos de Access, por ejemplo), puede crear su propio proveedor.

## <a name="creating-your-own-membership-provider"></a>Crear su propio proveedor de pertenencia

Para crear su propio proveedor de pertenencia, primero deberá crear una clase que hereda de la clase MembershipProvider. Si utilizas VB.NET, Visual Studio 2005 agregará el código auxiliar para todos los métodos que debe reemplazar. Si está utilizando C#, su hasta que agregue el código auxiliar.

Debe reemplazar lo siguiente:

- Propiedad ApplicationName
- ChangePassword (función)
- ChangePasswordQuestionAndAnswer (función)
- CreateUser (función)
- DeleteUser (función)
- Propiedad EnablePasswordReset
- Propiedad EnablePasswordRetrieval
- FindUsersByEmail (función)
- FindUsersByName (función)
- GetAllUsers (función)
- GetNumberOfUsersOnline (función)
- Función GetPassword
- GetUser (función)
- GetUserNameByEmail (función)
- Propiedad MaxInvalidPasswordAttempts
- Propiedad MinRequiredNonAlphanumericCharacters
- Propiedad MinRequiredPasswordLength
- Propiedad PasswordAttemptWindow
- Propiedad PasswordFormat
- Propiedad PasswordStrengthRegularExpression
- Propiedad RequiresQuestionAndAnswer
- Propiedad RequiresUniqueEmail
- ResetPassword (función)
- Unlock (función de usuario)
- UpdateUser (función)
- ValidateUser (función)

Thats bastante lista implementar como un programador de C#. Le resultará más fácil de crear la clase en VB.NET sin ninguna implementación y, a continuación, usar .NET Reflector u otra herramienta similar para convertir el código en C#.

La cadena de conexión y otras propiedades deben establecerse en sus valores predeterminados en el método Initialize. (El método Initialize se desencadena cuando el proveedor se carga en tiempo de ejecución). El segundo parámetro al método Initialize es de tipo System.Collections.Specialized.NameValueCollection y es una referencia a la &lt;agregar&gt; elemento que está asociado con su proveedor personalizado en el archivo web.config. Esa entrada es similar a lo siguiente:

[!code-xml[Main](membership/samples/sample6.xml)]

Este es un ejemplo del método de inicialización.

[!code-csharp[Main](membership/samples/sample7.cs)]

Para validar el usuario cuando envía el formulario de inicio de sesión, debe utilizar el método ValidateUser. Este método se activa cuando el usuario hace clic en el botón de inicio de sesión en el control de inicio de sesión. Deberá colocar el código que realiza la búsqueda de usuario en este método.

Como puede ver, escribir su propio proveedor de pertenencia no es difícil y le permite ampliar esta funcionalidad eficaz de ASP.NET 2.0.
