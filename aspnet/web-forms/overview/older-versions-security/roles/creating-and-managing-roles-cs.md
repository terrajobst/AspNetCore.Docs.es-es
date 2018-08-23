---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Crear y administrar Roles (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial examinan los pasos necesarios para configurar el marco de trabajo de Roles. A continuación, vamos a crear páginas web para crear y eliminar roles.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: 41cd8cef2582b730391e5221629e9bb35794c159
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837972"
---
<a name="creating-and-managing-roles-c"></a>Crear y administrar Roles (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) o [descargar PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Este tutorial examinan los pasos necesarios para configurar el marco de trabajo de Roles. A continuación, vamos a crear páginas web para crear y eliminar roles.


## <a name="introduction"></a>Introducción

En el <a id="_msoanchor_1"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-cs.md) tutorial hemos examinado el uso de la autorización de dirección URL para impedir que ciertos usuarios un conjunto de páginas y explorado declarativo y técnicas de programación para ajustar la funcionalidad de una página ASP.NET según el usuario visitante. Concesión de permisos de acceso a la página o la funcionalidad de un usuario por el usuario, sin embargo, puede convertirse en una pesadilla de mantenimiento en escenarios donde hay muchas cuentas de usuario o cuando los privilegios de los usuarios cambian con frecuencia. Siempre que un usuario obtiene o pierde la autorización para realizar una tarea determinada, el administrador debe actualizar las reglas de autorización de dirección URL adecuadas, el marcado declarativo y el código.

Normalmente es útil para clasificar a los usuarios en grupos o *roles* y, a continuación, aplicar permisos según el rol por rol. Por ejemplo, la mayoría de las aplicaciones web tienen un determinado conjunto de páginas o tareas que se han reservado solo para los usuarios administrativos. Mediante las técnicas aprendidas en la *autorización basada en usuario* tutorial, se agregaría las reglas de autorización de dirección URL adecuadas, marcado declarativo y el código para que las cuentas de usuario especificado realizar tareas administrativas. Pero si se ha agregado un nuevo administrador, o si es necesario un administrador existente con sus derechos de administración revocados, tendríamos que devolver y actualizar los archivos de configuración y las páginas web. Con los roles, sin embargo, podríamos crear una función que llama a los administradores y asignar los usuarios de confianza a la función de los administradores. A continuación, se recomienda agregar las reglas de autorización de dirección URL adecuadas, marcado declarativo y el código para que la función administradores realizar las diversas tareas administrativas. Con esta infraestructura en su lugar, agregar nuevos administradores para el sitio o eliminar los existentes es tan sencillo como incluir o quitar el usuario de la función Administradores. Ninguna configuración, marcado declarativo o cambios de código son necesarios.

ASP.NET ofrece un marco de Roles para definir roles y asociarlos a las cuentas de usuario. Con el marco de Roles, podemos crear y eliminar roles, agregar usuarios o quitar usuarios de un rol, determinar el conjunto de usuarios que pertenecen a un rol determinado y saber si un usuario pertenece a un rol determinado. Una vez que se ha configurado el marco de trabajo de Roles, nos podemos limitar el acceso a las páginas según el rol por rol mediante reglas de autorización de dirección URL y mostrar u ocultar información adicional o funcionalidad en una página basada en roles del usuario ha iniciado sesión actualmente.

Este tutorial examinan los pasos necesarios para configurar el marco de trabajo de Roles. A continuación, vamos a crear páginas web para crear y eliminar roles. En el <a id="_msoanchor_2"> </a> [ *asignar Roles a los usuarios* ](assigning-roles-to-users-cs.md) tutorial veremos cómo agregar y quitar usuarios de roles. Y, en el <a id="_msoanchor_3"> </a> [ *autorización basada en roles* ](role-based-authorization-cs.md) tutorial veremos cómo limitar el acceso a las páginas según el rol por rol y descubra cómo ajustar la función de la funcionalidad en la página en función del usuario visitante. Comencemos.

## <a name="step-1-adding-new-aspnet-pages"></a>Paso 1: Agregar nuevas páginas ASP.NET

En este tutorial y los dos siguientes analizaremos diversas funciones relacionadas con las funciones y capacidades. Necesitamos una serie de páginas ASP.NET para implementar los temas que se examinan a lo largo de estos tutoriales. Vamos a crear estas páginas y actualizar el mapa del sitio.

Empiece por crear una nueva carpeta en el proyecto denominado `Roles`. A continuación, agregue cuatro nuevas páginas de ASP.NET para la `Roles` carpeta, vincular cada página con el `Site.master` página maestra. Nombre de las páginas:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

En este momento, el Explorador de soluciones del proyecto debe ser similar a la pantalla se muestra en la figura 1.


[![Se han agregado cuatro nuevas páginas a la carpeta de Roles](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Figura 1**: cuatro nuevas páginas se han agregado a la `Roles` carpeta ([haga clic aquí para ver imagen en tamaño completo](creating-and-managing-roles-cs/_static/image3.png))


En este momento, cada página tendrá los dos controles de contenido, uno para cada uno de ContentPlaceHolders la página maestra: `MainContent` y `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Recuerde que el `LoginContent` marcado de forma predeterminada del ContentPlaceHolder muestra un vínculo para iniciar sesión o cerrar sesión del sitio, dependiendo de si el usuario está autenticado. La presencia de la `Content2` contenido de control en la página ASP.NET, sin embargo, reemplaza el marcado de la página maestra predeterminada. Como se explicó en <a id="_msoanchor_4"> </a> [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, reemplazar el marcado predeterminado es útil en las páginas donde no queremos mostrar relacionados con el inicio de sesión Opciones de la columna izquierda.

Para estas cuatro páginas, sin embargo, deseamos mostrar marcado de la página maestra predeterminada para el `LoginContent` ContentPlaceHolder. Por lo tanto, quite el marcado declarativo para el `Content2` control de contenido. Una vez hecho esto, cada uno de los cuatro marcado de la página debe contener un solo control de contenido.

Por último, vamos a actualizar el mapa del sitio (`Web.sitemap`) para incluir estas nuevas páginas web. Agregue el siguiente código XML después de la `<siteMapNode>` se ha agregado para los tutoriales de pertenencia.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Con el mapa del sitio actualizado, visite el sitio mediante un explorador. Como se muestra en la figura 2, el panel de navegación de la izquierda ahora incluye elementos para los tutoriales de Roles.


[![Se han agregado cuatro nuevas páginas a la carpeta de Roles](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Figura 2**: cuatro nuevas páginas se han agregado a la `Roles` carpeta ([haga clic aquí para ver imagen en tamaño completo](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Paso 2: Especificar y configurar el proveedor de Roles de Framework

Al igual que el marco de pertenencia, el marco de trabajo de Roles se basa en el modelo de proveedor. Como se describe en el <a id="_msoanchor_5"> </a> [ *conceptos básicos de seguridad y compatibilidad de ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) , tutorial de .NET Framework incluye tres proveedores de Roles integrados: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), y [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Esta serie de tutoriales se centra en la `SqlRoleProvider`, que utiliza una base de datos de Microsoft SQL Server como el almacén de roles.

Interiormente el marco de trabajo de Roles y `SqlRoleProvider` funcionan igual que el marco de pertenencia y `SqlMembershipProvider`. .NET Framework contiene un `Roles` clase que actúa como la API para el marco de trabajo de Roles. El `Roles` clase tiene métodos estáticos como `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`, y así sucesivamente. Cuando se invoca uno de estos métodos, el `Roles` clase delega la llamada en el proveedor configurado. El `SqlRoleProvider` funciona con las tablas específicas de cada rol (`aspnet_Roles` y `aspnet_UsersInRoles`) en la respuesta.

Para poder usar el `SqlRoleProvider` proveedor en nuestra aplicación, se debe especificar lo que la base de datos que se usará como el almacén. El `SqlRoleProvider` espera tener determinadas tablas de base de datos, vistas y procedimientos almacenados en el almacén de rol especificado. Estos objetos de base de datos necesarios se pueden agregar utilizando el [ `aspnet_regsql.exe` herramienta](https://msdn.microsoft.com/library/ms229862.aspx). En este momento ya tenemos una base de datos con el esquema necesario para el `SqlRoleProvider`. En el <a id="_msoanchor_6"> </a> [ *crear el esquema de pertenencia en SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) tutorial, creamos una base de datos denominada `SecurityTutorials.mdf` y usar `aspnet_regsql.exe` para agregar la aplicación servicios, que incluye los objetos de base de datos requeridos por la `SqlRoleProvider`. Por lo tanto, sólo debemos indicar al marco de Roles para habilitar la compatibilidad con el rol y usar el `SqlRoleProvider` con el `SecurityTutorials.mdf` como el almacén de roles de base de datos.

El marco de trabajo de Roles se configura mediante el &lt; `roleManager` &gt; elemento en la aplicación `Web.config` archivo. De forma predeterminada, está deshabilitada la compatibilidad con el rol. Para habilitarla, debe establecer el [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) del elemento `enabled` atributo `true` así:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

De forma predeterminada, todas las aplicaciones web tienen un proveedor de Roles denominado `AspNetSqlRoleProvider` de tipo `SqlRoleProvider`. Este proveedor predeterminado está registrado en `machine.config` (ubicado en `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

El proveedor `connectionStringName` atributo especifica el almacén de roles que se usa. El `AspNetSqlRoleProvider` proveedor establece este atributo en `LocalSqlServer`, que también se define en `machine.config` puntos y, de forma predeterminada, una base de datos de SQL Server 2005 Express Edition en el `App_Data` carpeta denominada `aspnet.mdf`.

Por lo tanto, si se habilita simplemente el marco de trabajo de funciones sin especificar ninguna información de proveedor en nuestra aplicación `Web.config` archivo, la aplicación utiliza el proveedor de Roles predeterminado registrado, `AspNetSqlRoleProvider`. Si el `~/App_Data/aspnet.mdf` base de datos no existe, el tiempo de ejecución ASP.NET creará automáticamente y agregue el esquema de servicios de aplicación. Sin embargo, no deseamos usar el `aspnet.mdf` base de datos; en su lugar, queremos usar el `SecurityTutorials.mdf` base de datos que ya hemos creado y agrega el esquema de servicios de aplicación a. Esta modificación se puede lograr de dos maneras:

- <strong>Especifique un valor para el</strong><strong>`LocalSqlServer`</strong><strong>nombre de la cadena de conexión en</strong><strong>`Web.config`</strong><strong>.</strong> Sobrescribiendo el `LocalSqlServer` valor de nombre de la cadena de conexión en `Web.config`, podemos usar el proveedor de Roles predeterminado registrado (`AspNetSqlRoleProvider`) y hacer que funcione correctamente con el `SecurityTutorials.mdf` base de datos. Para obtener más información sobre esta técnica, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog, [configurar servicios de aplicación de ASP.NET 2.0 para utilizar SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Agregar un nuevo proveedor registrado del tipo</strong><strong>`SqlRoleProvider`</strong><strong>y configurar su</strong><strong>`connectionStringName`</strong><strong>configuración para que apunte a la</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>base de datos.</strong> Éste es el enfoque recomienda y se utiliza en el <a id="_msoanchor_7"></a>[*crear el esquema de pertenencia en SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) tutorial y es el enfoque que usará en este tutorial también.

Agregue el siguiente marcado de configuración de Roles para el `Web.config` archivo. Este marcado registra un proveedor nuevo denominado `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

El marcado anterior se define la `SecurityTutorialsSqlRoleProvider` como el proveedor predeterminado (a través de la `defaultProvider` atributo el `<roleManager>` elemento). También establece la `SecurityTutorialsSqlRoleProvider`del `applicationName` si se establece en `SecurityTutorials`, que es el mismo `applicationName` valor utilizado por el proveedor de pertenencia (`SecurityTutorialsSqlMembershipProvider`). Mientras no se muestra aquí, el [ `<add>` elemento](https://msdn.microsoft.com/library/ms164662.aspx) para el `SqlRoleProvider` también puede contener un `commandTimeout` atributo para especificar la duración de tiempo de espera de la base de datos, en segundos. El valor predeterminado es 30.

Con esta marca de configuración en su lugar, estamos preparados empezar a usar la funcionalidad de rol dentro de nuestra aplicación.

> [!NOTE]
> El marcado de la configuración anterior se muestra cómo utilizar el &lt; `roleManager` &gt; del elemento `enabled` y `defaultProvider` atributos. Hay una serie de otros atributos que afectan a cómo el marco de Roles asocia información de roles de usuario por usuario. Examinaremos estos valores en el <a id="_msoanchor_8"> </a> [ *autorización basada en roles* ](role-based-authorization-cs.md) tutorial.


## <a name="step-3-examining-the-roles-api"></a>Paso 3: Examen de las funciones de API

Funcionalidad del marco de Roles se expone a través de la [ `Roles` clase](https://msdn.microsoft.com/library/system.web.security.roles.aspx), que contiene trece métodos estáticos para realizar operaciones basadas en rol. Cuando nos centramos en la creación y eliminación de roles en los pasos 4 y 6 usaremos el [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) y [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) métodos, que agrega o quita un rol del sistema.

Para obtener una lista de todos los roles del sistema, use la [ `GetAllRoles` método](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (consulte el paso 5). El [ `RoleExists` método](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) devuelve un valor booleano que indica si existe un rol especificado.

En el siguiente tutorial, examinaremos cómo asociar usuarios a roles. El `Roles` la clase [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), y [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) métodos de agregan uno o varios usuarios a uno o varios roles. Para quitar los usuarios de roles, use el [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), o [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) métodos.

En el <a id="_msoanchor_9"> </a> [ *autorización basada en roles* ](role-based-authorization-cs.md) tutorial veremos cómo mostrar u ocultar la funcionalidad basada en función del usuario con sesión iniciada actualmente. Para lograr esto, podemos usar el `Role` la clase [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), o [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) métodos.

> [!NOTE]
> Tenga en cuenta que se invoca siempre que uno de estos métodos, el `Roles` clase delega la llamada en el proveedor configurado. En nuestro caso, esto significa que la llamada se envía a la `SqlRoleProvider`. El `SqlRoleProvider` , a continuación, realiza la operación de base de datos adecuada en función del método invocado. Por ejemplo, el código `Roles.CreateRole("Administrators")` da como resultado la `SqlRoleProvider` ejecutando el `aspnet_Roles_CreateRole` procedimiento almacenado, que inserta un nuevo registro en el `aspnet_Roles` denominado Administradores de tabla.


El resto de este tutorial se examina utilizando el `Roles` la clase `CreateRole`, `GetAllRoles`, y `DeleteRole` métodos para administrar los roles en el sistema.

## <a name="step-4-creating-new-roles"></a>Paso 4: Creación de nuevas funciones

Roles ofrecen una manera arbitrariamente los usuarios del grupo y esta agrupación se usa normalmente para que una manera más cómoda aplicar las reglas de autorización. Pero para poder usar funciones como un mecanismo de autorización primero necesitamos definir los roles que existen en la aplicación. Lamentablemente, ASP.NET no incluye un control CreateRoleWizard. Para agregar nuevos roles es necesario crear una interfaz de usuario adecuado e invocar la API de Roles de nosotros mismos. La buena noticia es que esto es muy fácil de conseguir.

> [!NOTE]
> Aunque no hay ningún control CreateRoleWizard Web, es el [ASP.NET Web Site Administration Tool](https://msdn.microsoft.com/library/ms228053.aspx), que es una aplicación ASP.NET local diseñada para ayudar a ver y administrar la configuración de la aplicación web. Sin embargo, no soy un gran defensor de la herramienta de administración de sitios Web de ASP.NET por dos motivos. En primer lugar, es un poco con errores y la experiencia del usuario deja mucho que desear. En segundo lugar, la herramienta de administración de sitios Web de ASP.NET está diseñada para que funcione solo localmente, lo que significa que tendrá que crear su propia función de las páginas web de administración si tiene que administrar roles en un sitio activo de forma remota. Para estos dos motivos, este tutorial y los restantes se centran en crear el rol es necesario que las herramientas de administración en una página web, en lugar de depender de la herramienta de administración de sitios Web de ASP.NET.


Abra el `ManageRoles.aspx` página en el `Roles` carpeta y agregue un cuadro de texto y un control Button Web a la página. Establecer el control de cuadro de texto `ID` propiedad `RoleName` y el botón `ID` y `Text` propiedades a `CreateRoleButton` y Create Role, respectivamente. En este momento, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

A continuación, haga doble clic en el `CreateRoleButton` botón control en el diseñador para crear un `Click` controlador de eventos y, a continuación, agregue el código siguiente:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

El código anterior comienza asignando el nombre de rol recortada especificado en el `RoleName` cuadro de texto para el `newRoleName` variable. A continuación, el `Roles` la clase `RoleExists` método se llama para determinar si el rol `newRoleName` ya existe en el sistema. Si el rol no existe, se crea mediante una llamada a la `CreateRole` método. Si el `CreateRole` método se pasa un nombre de rol que ya existe en el sistema, un `ProviderException` es una excepción. Es por esta razón el código comprueba primero para asegurarse de que el rol no existe ya en el sistema antes de llamar a `CreateRole`. El `Click` controlador de eventos concluya borrando el `RoleName` del cuadro de texto `Text` propiedad.

> [!NOTE]
> Quizás se pregunte lo que ocurrirá si el usuario no escribe ningún valor en el `RoleName` cuadro de texto. Si el valor pasado en el `CreateRole` método es `null` o se produce una cadena vacía, una excepción. Del mismo modo, si el nombre del rol contiene una coma, se produce una excepción. Por lo tanto, la página debe contener los controles de validación para asegurarse de que el usuario especifica un rol y que no contiene ninguna coma. Dejo como ejercicio para el lector.


Vamos a crear un rol denominado Administradores. Visite el `ManageRoles.aspx` página a través de un explorador, escriba Administradores en el cuadro de texto (consulte la figura 3) y, a continuación, haga clic en el botón Create Role.


[![Crear un rol de administradores](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Figura 3**: crear un rol de administradores ([haga clic aquí para ver imagen en tamaño completo](creating-and-managing-roles-cs/_static/image9.png))


¿Qué ocurre? Se produce un postback, pero no hay ninguna indicación visual que el rol realmente se ha agregado al sistema. Actualizaremos esta página en el paso 5 para incluir comentarios visuales. Por ahora, sin embargo, puede comprobar que se creó el rol, vaya a la `SecurityTutorials.mdf` base de datos y muestra los datos de la `aspnet_Roles` tabla. Como se muestra en la figura 4, el `aspnet_Roles` tabla contiene un registro para los roles de administradores recién agregados.


[![La tabla aspnet_Roles tiene una fila para los administradores](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Figura 4**: el `aspnet_Roles` tabla tiene una fila para los administradores ([haga clic aquí para ver imagen en tamaño completo](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Paso 5: Mostrar los Roles en el sistema

Vamos a aumentar la `ManageRoles.aspx` página debe incluir una lista de los roles actuales en el sistema. Para ello, agregue un control GridView a la página y establezca su `ID` propiedad `RoleList`. A continuación, agregue un método a la clase de código subyacente de la página denominada `DisplayRolesInGrid` con el código siguiente:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

El `Roles` la clase `GetAllRoles` método devuelve todos los roles del sistema como una matriz de cadenas. Esta matriz de cadenas se enlaza a la GridView. Para enlazar la lista de roles en GridView cuando se carga la página por primera vez, es necesario llamar a la `DisplayRolesInGrid` método desde la página `Page_Load` controlador de eventos. El código siguiente llama a este método cuando primero se visita la página, pero no en los postbacks subsiguientes.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Con este código en su lugar, visite la página a través de un explorador. Como se muestra en la figura 5, debería ver una cuadrícula con una sola columna de la etiqueta de elemento. La cuadrícula incluye una fila para la función de los administradores que se agrega en el paso 4.


[![El control GridView muestra las funciones en una sola columna](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Figura 5**: GridView muestra las funciones en una sola columna ([haga clic aquí para ver imagen en tamaño completo](creating-and-managing-roles-cs/_static/image15.png))


El control GridView muestra una única columna elemento etiquetada, porque la GridView `AutoGenerateColumns` propiedad está establecida en True (valor predeterminado), lo que hace que el control GridView crear automáticamente una columna para cada propiedad en su `DataSource`. Una matriz tiene una propiedad única que representa los elementos de la matriz, por lo tanto, la única columna de GridView.

Visualización de datos con un control GridView, prefiero definir mis columnas explícitamente en lugar de hacer que se generen implícitamente por el control GridView. Al definir explícitamente las columnas es mucho más fácil dar formato a los datos, reorganizar las columnas y realizar otras tareas habituales. Por lo tanto, vamos a actualizar marcado declarativo de GridView para que sus columnas se definen explícitamente.

Comience por establecer la GridView `AutoGenerateColumns` propiedad en False. A continuación, agregue un TemplateField a la cuadrícula, establezca su `HeaderText` propiedad a los Roles y configurar su `ItemTemplate` para que se muestre el contenido de la matriz. Para ello, agregue un control de etiqueta Web denominado `RoleNameLabel` a la `ItemTemplate` y enlazar su `Text` propiedad `Container.DataItem`.

Estas propiedades y el `ItemTemplate`del contenido se puede establecer mediante declaración o a través de cuadro de diálogo campos de GridView y la interfaz de edición de plantillas. Para llegar a los campos de cuadro de diálogo, haga clic en el vínculo Editar columnas en la etiqueta inteligente de GridView. A continuación, desactive la casilla de los campos de generar automáticamente para establecer el `AutoGenerateColumns` propiedad en False y agregar TemplateField en el control GridView, estableciendo su `HeaderText` propiedad al rol. Para definir el `ItemTemplate`del contenido, elija la opción de editar plantillas de etiqueta inteligente de GridView. Arrastre un control Web Label el `ItemTemplate`, establezca su `ID` propiedad `RoleNameLabel`y configurar sus opciones de enlace de datos para que su `Text` propiedad está enlazada a `Container.DataItem`.

Independientemente de qué método que se utilice, marcado declarativo de GridView resultante debe ser similar al siguiente cuando haya terminado.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Contenido de la matriz se muestra utilizando la sintaxis de enlace de datos `<%# Container.DataItem %>`. Una descripción detallada de por qué esta sintaxis se utiliza al mostrar el contenido de una matriz enlaza a GridView queda fuera del ámbito de este tutorial. Para obtener más información sobre este asunto, consulte [enlazar una matriz de escalar a un Control de datos Web](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Actualmente, el `RoleList` GridView solo está enlazado a la lista de roles cuando primero se visita la página. Es necesario actualizar la cuadrícula cuando se agregue un nuevo rol. Para lograr esto, actualice el `CreateRoleButton` del botón `Click` controlador de eventos, por lo que llama el `DisplayRolesInGrid` método si se crea un nuevo rol.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Ahora, cuando el usuario agrega un nuevo rol de la `RoleList` GridView muestra la función de agregado solo en el postback, proporcionar comentarios visuales que el rol se creó correctamente. Para ilustrar esto, visite la `ManageRoles.aspx` página a través de un explorador y agregue un rol denominado supervisores. Al hacer clic en el botón Create Role, surgirán una devolución de datos y la cuadrícula se actualizará para incluir los administradores, así como el nuevo rol, los supervisores.


[![El rol de los supervisores tiene se han agregado](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Figura 6**: The Role supervisores se ha agregado ([haga clic aquí para ver imagen en tamaño completo](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Paso 6: Eliminación de Roles

En este momento en que un usuario puede crear un nuevo rol y ver todos los roles existentes desde el `ManageRoles.aspx` página. Vamos a permitir a los usuarios también eliminar roles. El `Roles.DeleteRole` método tiene dos sobrecargas:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -elimina la función *roleName*. Se produce una excepción si la función contiene a uno o más miembros.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -elimina la función *roleName*. Si *throwOnPopulateRole* es `true`, a continuación, se produce una excepción si la función contiene uno o más miembros. Si *throwOnPopulateRole* es `false`, a continuación, se elimina el rol si contiene algún miembro o no. Internamente, el `DeleteRole(roleName)` llamadas al método `DeleteRole(roleName, true)`.

El `DeleteRole` método también producirá una excepción si *roleName* es `null` o una cadena vacía o si *roleName* contiene una coma. Si *roleName* no existe en el sistema, `DeleteRole` produce un error silencioso, sin provocar una excepción.

Vamos a aumentar el control GridView en `ManageRoles.aspx` para incluir una eliminación botón que, al hacer clic, elimina el rol seleccionado. Empiece agregando un botón Eliminar en el control GridView, vaya al cuadro de diálogo campos y agregar un botón de eliminación, que se encuentra en la opción CommandField. Realizar la eliminación de botón de la columna izquierda y establezca su `DeleteText` propiedad al eliminar el rol.


[![Agregar un botón Eliminar en el control RoleList GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Figura 7**: agregar un botón de eliminar el `RoleList` GridView ([haga clic aquí para ver imagen en tamaño completo](creating-and-managing-roles-cs/_static/image21.png))


Después de agregar el botón Eliminar, el marcado declarativo de la GridView debe ser similar al siguiente:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

A continuación, cree un controlador de eventos para el control de GridView `RowDeleting` eventos. Este es el evento que se inicia en el postback cuando se hace clic en el botón Eliminar función. Agregue el siguiente código al controlador de eventos.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

El código se inicia haciendo mediante programación el `RoleNameLabel` Web control de la fila cuyo botón Eliminar función se hizo clic. El `Roles.DeleteRole` , a continuación, se invoca el método, pasando el `Text` de la `RoleNameLabel` y `false`, lo que al eliminar el rol independientemente de si hay usuarios asociados a la función. Por último, el `RoleList` GridView se actualiza para que el rol recién eliminados ya no aparece en la cuadrícula.

> [!NOTE]
> El botón Eliminar función no requiere a algún tipo de confirmación del usuario antes de eliminar el rol. Una de las maneras más fáciles para confirmar una acción es a través de un cuadro de diálogo de confirmación del lado cliente. Para obtener más información sobre esta técnica, consulte [adición del lado cliente confirmación cuando se elimina](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Resumen

Muchas aplicaciones web tienen ciertas reglas de autorización o funcionalidad de nivel de página que solo está disponible para determinadas clases de usuarios. Por ejemplo, puede haber un conjunto de páginas web que solo los administradores pueden tener acceso. En lugar de definir estas reglas de autorización de usuario por usuario, a menudo es más útil definir las reglas basándose en una función. Es decir, en lugar de permitir explícitamente a los usuarios tener acceso a las páginas web administrativas Scott y Jisun, un enfoque más fácil de mantener permitir que los miembros del rol de administradores para tener acceso a estas páginas y, a continuación, para denotar cómo Scott y Jisun como los usuarios que pertenecen a la Rol de administradores.

El marco de trabajo de Roles facilita la creación y administración de roles. En este tutorial hemos visto cómo configurar el marco de Roles para utilizar el `SqlRoleProvider`, que utiliza una base de datos de Microsoft SQL Server como el almacén de roles. También hemos creado una página web que se enumeran los roles existentes en el sistema y permite que se creen nuevas funciones y los existentes se eliminarán. En los tutoriales posteriores, veremos cómo asignar a usuarios a roles y cómo aplicar la autorización basada en roles.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Examen de ASP.NET del 2.0 perfil, Roles y pertenencia](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Cómo: Usar el Administrador de roles en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Proveedores de funciones](https://msdn.microsoft.com/library/aa478950.aspx)
- [Implementar su propia herramienta de administración del sitio Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentación técnica de la `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)
- [Mediante la pertenencia y las API de administrador de rol](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial incluyen Alicja Maziarz Suchi Banerjee y Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](assigning-roles-to-users-cs.md)
