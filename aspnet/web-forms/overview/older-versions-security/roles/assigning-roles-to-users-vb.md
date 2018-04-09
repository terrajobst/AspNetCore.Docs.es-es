---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: Asignar Roles a los usuarios (VB) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial crearemos dos páginas ASP.NET para ayudar con la administración de los que los usuarios pertenecen a qué funciones. La primera página incluirá instalaciones para ver qué...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: 959a73f53d4fdb114f222fe8bc830876b76c9d9e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="assigning-roles-to-users-vb"></a>Asignar Roles a los usuarios (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) o [descarga de PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> En este tutorial crearemos dos páginas ASP.NET para ayudar con la administración de los que los usuarios pertenecen a qué funciones. La primera página incluye instalaciones para ver qué usuarios pertenecen a un rol determinado, los roles que pertenece un usuario determinado y la capacidad de asignar o quitar un usuario determinado de un rol determinado. En la segunda página aumentaremos el control CreateUserWizard para que incluya un paso para especificar las funciones a la que pertenece el usuario recién creado. Esto es útil en escenarios donde un administrador es capaz de crear nuevas cuentas de usuario.


## <a name="introduction"></a>Introducción

El <a id="_msoanchor_1"> </a> [tutorial anterior](creating-and-managing-roles-vb.md) examina el marco de trabajo de Roles y la `SqlRoleProvider`; hemos visto cómo utilizar la `Roles` clase para crear, recuperar y eliminar roles. Además de crear y eliminar roles, necesitamos poder asignar o quitar usuarios de un rol. Por desgracia, ASP.NET no se distribuye con todos los controles Web para administrar lo que los usuarios pertenecen a qué funciones. En su lugar, deberemos crear nuestras propia páginas ASP.NET para administrar estas asociaciones. Las buenas noticias son que agregar y quitar usuarios de roles es bastante fácil. La `Roles` clase contiene una serie de métodos para agregar uno o más usuarios a uno o varios roles.

En este tutorial crearemos dos páginas ASP.NET para ayudar con la administración de los que los usuarios pertenecen a qué funciones. La primera página incluye instalaciones para ver qué usuarios pertenecen a un rol determinado, los roles que pertenece un usuario determinado y la capacidad de asignar o quitar un usuario determinado de un rol determinado. En la segunda página aumentaremos el control CreateUserWizard para que incluya un paso para especificar las funciones a la que pertenece el usuario recién creado. Esto es útil en escenarios donde un administrador es capaz de crear nuevas cuentas de usuario.

Comencemos.

## <a name="listing-what-users-belong-to-what-roles"></a>Lista de los usuarios pertenecen a qué funciones

La primera regla de negocio para este tutorial consiste en crear una página web desde el que los usuarios se pueden asignar a roles. Antes de que se hacen referencia a nosotros mismos con cómo asignar a usuarios a roles, vamos a concentrarse en primer lugar sobre cómo determinar qué usuarios pertenecen a los roles. Hay dos maneras de mostrar esta información: "rol" o "por"usuario. Se pueden permitir a los visitantes seleccionar un rol y, a continuación, mostrarlos todos los usuarios que pertenecen al rol (la pantalla "por rol"), o se puede pedir el visitante para seleccionar un usuario y, a continuación, mostrarlos en los roles asignados a ese usuario (la pantalla "por usuario").

La vista "por"función es útil en los casos donde el visitante desea conocer el conjunto de usuarios que pertenecen a un rol determinado; la vista "por usuario" es ideal cuando el visitante necesita conocer los roles de un usuario determinado. Supongamos que nuestra página de incluya "por rol" y "por usuario" interfaces.

Comenzaremos con la creación de la interfaz "por usuario". Esta interfaz constará de una lista desplegable y una lista de casillas de verificación. La lista desplegable se llenará con el conjunto de usuarios en el sistema; las casillas de verificación enumerará los roles. Si se selecciona un usuario de la lista desplegable comprobará que esos roles que pertenece el usuario. La persona que visita la página puede, a continuación, active o desactive las casillas de verificación para agregar o quitar el usuario seleccionado de los roles correspondientes.

> [!NOTE]
> Con una lista desplegable lista a las cuentas de usuario no es una opción ideal para los sitios Web donde puede haber cientos de cuentas de usuario. Una lista desplegable está diseñada para permitir que un usuario seleccionar un elemento de una lista relativamente corta de opciones. Deja de ser rápidamente difícil de manejar a medida que aumenta el número de elementos de lista. Si va a crear un sitio Web que tendrá el número potencialmente grande de las cuentas de usuario, puede que desee considerar el uso de una interfaz de usuario alternativa, como un control GridView paginable o una interfaz filtrable que enumera pide al visitante elegir una letra y, a continuación, solo Muestra los usuarios cuyo nombre de usuario comienza por la letra seleccionada.


## <a name="step-1-building-the-by-user-user-interface"></a>Paso 1: Creación de la interfaz de usuario "Por usuario"

Abra la `UsersAndRoles.aspx` página. En la parte superior de la página, agregue un control de etiqueta Web denominado `ActionStatus` y borrar su `Text` propiedad. Esta etiqueta se utilizará para proporcionar comentarios sobre las acciones realizadas, mostrar mensajes como, "Tito de usuario se ha agregado a la función Administradores" o "Jisun de usuario se quitó de la función de los supervisores." Para hacer estos mensajes se resaltan, Establece la etiqueta `CssClass` propiedad a "Importante".

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

A continuación, agregue la siguiente definición de clase CSS para el `Styles.css` hojas de estilo:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Esta definición CSS indica al explorador para mostrar la etiqueta con una fuente grande, roja. La figura 1 muestra este efecto mediante el Diseñador de Visual Studio.


[![Propiedad de la etiqueta CssClass da como resultado una fuente grande, roja](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Figura 1**: la etiqueta `CssClass` resultados de la propiedad en una grande, fuente de color rojo ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image3.png))


A continuación, agregue un DropDownList a la página, establezca su `ID` propiedad `UserList`y establezca su `AutoPostBack` propiedad en True. Usaremos esta DropDownList para enumerar todos los usuarios en el sistema. Este DropDownList se enlazará a una colección de objetos de MembershipUser. Dado que deseamos DropDownList para mostrar la propiedad de nombre de usuario del objeto MembershipUser (y usarlo como el valor de los elementos de lista), establezca la DropDownList `DataTextField` y `DataValueField` propiedades para "UserName".

Debajo DropDownList, agregue un repetidor denominado `UsersRoleList`. Este repetidor enumerará todos los roles en el sistema como una serie de casillas de verificación. Definir el repetidor `ItemTemplate` mediante el siguiente marcado declarativo:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

El `ItemTemplate` marcado incluye un control de casilla de verificación Web único denominado `RoleCheckBox`. La casilla de verificación `AutoPostBack` propiedad está establecida en True y la `Text` propiedad está enlazada a `Container.DataItem`. La razón por la sintaxis de enlace de datos es simplemente `Container.DataItem` es porque el marco de trabajo de Roles devuelve la lista de nombres de función como una matriz de cadenas, y es esta matriz de cadenas que se enlace el repetidor. Una descripción detallada de por qué se utiliza esta sintaxis para mostrar el contenido de una matriz que se enlaza a un control Web de datos queda fuera del ámbito de este tutorial. Para obtener más información sobre este asunto, consulte [una matriz escalar de enlace a un Control Web de datos](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

En este momento marcado declarativo de la interfaz de "por usuario" debe ser similar al siguiente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Ahora estamos listos para escribir el código para enlazar el conjunto de cuentas de usuario a los controles DropDownList y el conjunto de roles para repetidor. En la clase de código subyacente de la página, agregue un método denominado `BindUsersToUserList` y otro denominado `BindRolesList`, utilizando el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

El `BindUsersToUserList` método recupera todas las cuentas de usuario en el sistema a través de la [ `Membership.GetAllUsers` método](https://msdn.microsoft.com/library/dy8swhya.aspx). Esto devuelve un [ `MembershipUserCollection` objeto](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), que es una colección de [ `MembershipUser` instancias](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Esta colección se enlaza a la `UserList` DropDownList. El `MembershipUser` instancias que utilizan la colección contienen una gran variedad de propiedades, como `UserName`, `Email`, `CreationDate`, y `IsOnline`. Con el fin de indicar a DropDownList para mostrar el valor de la `UserName` propiedad, asegúrese de que el `UserList` del DropDownList `DataTextField` y `DataValueField` propiedades se han establecido en "UserName".

> [!NOTE]
> El `Membership.GetAllUsers` método tiene dos sobrecargas: una que no acepta ningún parámetro de entrada y devuelve todos los usuarios y otra que acepta valores enteros para el índice de la página y el tamaño de página y devuelve solo el subconjunto especificado de los usuarios. Cuando hay grandes cantidades de cuentas de usuario que se muestran en un elemento de la interfaz de usuario paginable, la segunda sobrecarga puede utilizarse para hojear más eficazmente los usuarios debido a que devuelve sólo el subconjunto preciso de las cuentas de usuario en lugar de todos ellos.


El `BindRolesToList` método se inicia mediante una llamada a la `Roles` la clase [ `GetAllRoles` método](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), que devuelve una matriz de cadenas que contiene los roles en el sistema. Esta matriz de cadenas, a continuación, se enlaza a repetidor.

Por último, es necesario llamar a estos dos métodos cuando se carga la página por primera vez. Agregue el código siguiente al controlador de eventos `Page_Load`:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Con este código en su lugar, tómese un momento para visitar la página a través de un explorador; la pantalla debe ser similar a la figura 2. Todas las cuentas de usuario se rellenan en la lista desplegable y, por debajo de ésta, cada rol aparece como una casilla de verificación. Dado que hemos establecido el `AutoPostBack` propiedades de las casillas de verificación y DropDownList en True, cambiar el usuario seleccionado o activación o desactivación de un rol provoca una devolución de datos. Sin embargo, se realiza ninguna acción porque aún es necesario escribir código para controlar estas acciones. Abordaremos estas tareas en las dos secciones siguientes.


[![La página muestra los usuarios y Roles](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Figura 2**: la página muestra los usuarios y Roles ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Comprobación de los Roles seleccionado que pertenece el usuario

Cuando se carga la página por primera vez o cada vez que el visitante selecciona un nuevo usuario de la lista desplegable, deberá actualizar el `UsersRoleList`de casillas de verificación para que se activa una casilla de un rol determinado solo si el usuario seleccionado pertenece a ese rol. Para ello, cree un método denominado `CheckRolesForSelectedUser` con el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

El código anterior empieza por determinar quién es el usuario seleccionado. A continuación, utiliza la clase de Roles [ `GetRolesForUser(userName)` método](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) para devolver el conjunto del usuario especificado de funciones como una matriz de cadenas. A continuación, se enumeran los elementos del repetidor y cada elemento `RoleCheckBox` casilla mediante programación se hace referencia. Se activa la casilla solo si está dentro de la función que se corresponde con el `selectedUsersRoles` matriz de cadenas.

> [!NOTE]
> El `Linq.Enumerable.Contains(Of String)(...)` sintaxis no se compilará si usa ASP.NET versión 2.0. El `Contains(Of String)` método forma parte de la [biblioteca LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), que es nueva en ASP.NET 3.5. Si aún utiliza la versión 2.0 de ASP.NET, use la [ `Array.IndexOf(Of String)` método](https://msdn.microsoft.com/library/eha9t187.aspx) en su lugar.


El `CheckRolesForSelectedUser` método debe llamarse en dos casos: cuando se carga la página por primera vez y siempre que el `UserList` se cambia el índice seleccionado del DropDownList. Por consiguiente, llamar a este método desde el `Page_Load` controlador de eventos (después de las llamadas a `BindUsersToUserList` y `BindRolesToList`). Además, crear un controlador de eventos para el DropDownList `SelectedIndexChanged` evento y llamar a este método desde allí.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Con este código en su lugar, puede probar la página a través del explorador. Sin embargo, dado que la `UsersAndRoles.aspx` página actualmente no tiene la capacidad para asignar usuarios a roles, ningún usuario tiene roles. Se creará la interfaz para asignar usuarios a roles en un momento, por lo que puede tardar una palabra que funciona este código y compruebe que lo hace más tarde, o puede agregar manualmente los usuarios a los roles mediante la inserción de registros en la `aspnet_UsersInRoles` tabla para probar este functi onality ahora.

### <a name="assigning-and-removing-users-from-roles"></a>Asignar y quitar usuarios de Roles

Cuando el visitante comprueba o desactiva una casilla de verificación en la `UsersRoleList` repetidor tenemos que agregar o quitar el usuario seleccionado de la función correspondiente. La casilla de verificación `AutoPostBack` propiedad actualmente se establece en True, lo que provoca una devolución de datos en cualquier momento una casilla de verificación del repetidor está activada o desactivada. En resumen, es necesario crear un controlador de eventos para la casilla de verificación `CheckChanged` eventos. Puesto que la casilla de verificación está en un control de repetidor, es necesario agregar manualmente la mecánica de controlador de eventos. Empiece por agregar el controlador de eventos a la clase de código subyacente como un `Protected` método, de este modo:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Se devolverá para escribir el código para este controlador de eventos en un momento. Pero primero vamos a completar la mecánica de control de eventos. En la casilla de verificación dentro del repetidor `ItemTemplate`, agregar `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Esta sintaxis conecta el `RoleCheckBox_CheckChanged` controlador de eventos para el `RoleCheckBox`de `CheckedChanged` eventos.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

La última tarea consiste en completar la `RoleCheckBox_CheckChanged` controlador de eventos. Se debe iniciar por hacer referencia al control de casilla de verificación que generó el evento porque esta instancia de la casilla de verificación nos indica qué rol se ha comprobado o a través de su `Text` y `Checked` propiedades. Con esta información junto con el nombre de usuario del usuario seleccionado, se agregar o quitar el usuario de la función a través de la `Roles` la clase [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) o [ `RemoveUserFromRole` método](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

El código anterior se inicia haciendo referencia mediante programación a la casilla de verificación que generó el evento, que está disponible a través de la `sender` parámetro de entrada. Si se activa la casilla de verificación, el usuario seleccionado se agrega a la función especificada, en caso contrario que se quitan de la función. En cualquier caso, el `ActionStatus` etiqueta muestra un mensaje con un resumen de la acción que se ha realizado.

Tómese un momento para probar esta página a través de un explorador. Seleccione usuario Tito y, a continuación, agregue a Tito a los administradores y los supervisores de roles.


[![Se ha agregado Tito a los administradores y los supervisores Roles](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Figura 3**: Tito se ha agregado a los administradores y los Roles de los supervisores ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image9.png))


A continuación, seleccione usuario Bruce en la lista desplegable. Hay una devolución de datos y se actualizan las casillas de verificación del repetidor a través de la `CheckRolesForSelectedUser`. Puesto que Bruce aún no pertenecen a ningún rol, las dos casillas están desactivadas. A continuación, agregar a Bruce a la función de los supervisores.


[![Bruce se ha agregado a la función de los supervisores](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Figura 4**: Bruce se ha agregado a la función de los supervisores ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image12.png))


Para comprobar la funcionalidad de la `CheckRolesForSelectedUser` (método), seleccione un usuario que no sean Tito o Bruce. Tenga en cuenta cómo las casillas de verificación están desactivadas automáticamente, lo que indica que no pertenece a ningún rol. Volver a Tito. Las casillas de los administradores y los supervisores deben comprobarse.

## <a name="step-2-building-the-by-roles-user-interface"></a>Paso 2: Crear la interfaz de usuario "Por Roles"

En este momento se ha completado la interfaz "por"usuarios y está listos para comenzar la superación de la interfaz "por roles". La interfaz "por roles" pide al usuario que seleccione un rol de una lista desplegable y, a continuación, muestra el conjunto de usuarios que pertenecen a ese rol en un control GridView.

Agregue otro control DropDownList para el `UsersAndRoles.aspx page`. Coloque este uno bajo el control de repetidor, asígnele el nombre `RoleList`y establezca su `AutoPostBack` propiedad en True. Por debajo de ésta, agregar un control GridView y asígnele el nombre `RolesUserList`. Este GridView mostrará una lista de los usuarios que pertenecen al rol seleccionado. Establecer la GridView `AutoGenerateColumns` propiedad en False, agregar TemplateField a la cuadrícula `Columns` colección y establezca su `HeaderText` propiedad a "Usuarios". Definir el TemplateField `ItemTemplate` para que se muestre el valor de la expresión de enlace de datos `Container.DataItem` en el `Text` propiedad de una etiqueta denominada `UserNameLabel`.

Después de agregar y configurar el control GridView, marcado declarativo de la interfaz de "por"función debe ser similar al siguiente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

Es necesario rellenar el `RoleList` DropDownList con el conjunto de roles en el sistema. Para lograr esto, actualice la `BindRolesToList` método por lo que se enlaza la matriz de cadena devuelta por la `Roles.GetAllRoles` método a la `RolesList` DropDownList (así como el `UsersRoleList` repetidor).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Las dos últimas líneas en el `BindRolesToList` método se han agregado al enlazar el conjunto de roles para la `RoleList` DropDownList (control). Figura 5 se muestra el resultado final cuando se ven a través de un explorador: una lista desplegable que se rellena con los roles del sistema.


[![Los Roles se muestran en RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Figura 5**: los Roles se mostrarán en el `RoleList` DropDownList ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Mostrar los usuarios que pertenecen al rol seleccionado

Cuando se carga la página por primera vez o cuando se selecciona un nuevo rol de la `RoleList` DropDownList, es necesario mostrar la lista de usuarios que pertenecen a ese rol en GridView. Crear un método denominado `DisplayUsersBelongingToRole` con el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Este método inicia obteniendo el rol seleccionado desde el `RoleList` DropDownList. A continuación, utiliza el [ `Roles.GetUsersInRole(roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) para recuperar una matriz de cadenas de los nombres de usuario de los usuarios que pertenecen a ese rol. Esta matriz se enlaza a la `RolesUserList` GridView.

Este método debe llamarse en dos casos: cuando se carga la página por primera vez y cuando el rol seleccionado en el `RoleList` DropDownList cambios. Por lo tanto, actualizar la `Page_Load` controlador de eventos para que este método se invoca después de llamar a `CheckRolesForSelectedUser`. A continuación, cree un controlador de eventos para el `RoleList`de `SelectedIndexChanged` eventos y llamar a este método a partir de ahí, demasiado.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Con este código en su lugar, el `RolesUserList` GridView debe mostrar los usuarios que pertenecen a la función seleccionada. Como se muestra en la figura 6, el rol de los supervisores consta de dos miembros: Bruce y Tito.


[![GridView enumera aquellos usuarios que pertenecen al rol seleccionado](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Figura 6**: el GridView muestra los usuarios que pertenecen al rol seleccionado ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Quitar usuarios del rol seleccionado

Vamos a aumentar la `RolesUserList` GridView para que incluya una columna de "quitar" botones. Haga clic en el botón "Quitar" para un usuario determinado quitará de ese rol.

Empiece agregando un campo de botón de eliminación en GridView. Asegúrese de este campo se muestra como la izquierda más archivada y cambiar su `DeleteText` propiedad en "Delete" (valor predeterminado) para "Eliminar".


[![Agregar el](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Figura 7**: agregar el botón "Eliminar" a la GridView ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image21.png))


Cuando se hace clic en el botón "Quitar" tiene lugar una devolución de datos y la GridView `RowDeleting` evento se desencadena. Es necesario crear un controlador de eventos para este evento y escribir código que se quita el usuario de la función seleccionada. Crear el controlador de eventos y, a continuación, agregue el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

El código se inicia al determinar el nombre del rol seleccionado. Después, mediante programación las referencias el `UserNameLabel` control de la fila cuyo botón "Quitar" se hizo clic con el fin de determinar el nombre de usuario del usuario para quitar. El usuario, a continuación, se quita de la función mediante una llamada a la `Roles.RemoveUserFromRole` método. El `RolesUserList` GridView, a continuación, se actualiza y se muestra un mensaje a través de la `ActionStatus` control de etiqueta.

> [!NOTE]
> El botón "Eliminar" no requiere a ningún tipo de confirmación del usuario antes de quitar el usuario de la función. Se puede invitar a agregar cierto nivel de confirmación del usuario. Una de las formas más sencillas para confirmar una acción es a través de un cuadro de diálogo de confirmación del lado cliente. Para obtener más información sobre esta técnica, consulte [Agregar cliente confirmación cuando se elimina](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Figura 8 muestra la página después de quitar usuario Tito desde el grupo de los supervisores.


[![Lamentablemente, Tito ya No es un Supervisor](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Figura 8**: Lamentablemente, Tito ya No es un Supervisor ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Adición de nuevos usuarios al rol seleccionado

Junto con la eliminación de usuarios del rol seleccionado, el visitante a esta página también deberían poder agregar un usuario al rol seleccionado. La mejor interfaz para agregar un usuario al rol seleccionado depende del número de cuentas de usuario que se espera que sean. Si su sitio Web alojará unos pocos cuentas de usuario docenas o menos, se podría utilizar un DropDownList aquí. Si puede haber miles de cuentas de usuario, desearía incluir una interfaz de usuario que permite el visitante para desplazarse a través de las cuentas, busque una cuenta determinada, o filtrar las cuentas de usuario de algún otro modo.

Para esta página permite que usen una sencilla interfaz que funciona independientemente del número de cuentas de usuario en el sistema. Es decir, se usará un cuadro de texto, preguntar el visitante para escribir en el nombre de usuario del usuario que desea agregar a la función seleccionada. Si no existe ningún usuario con ese nombre, o si el usuario ya es miembro del rol, se mostrará un mensaje en `ActionStatus` etiqueta. Pero si el usuario existe y no es un miembro del rol, se deberá agregarlos al rol y actualizar la cuadrícula.

Agregue un cuadro de texto y un botón debajo de GridView. Establezca el cuadro de texto `ID` a `UserNameToAddToRole` y establezca el botón `ID` y `Text` propiedades para `AddUserToRoleButton` y "Agregar al rol de usuario", respectivamente.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

A continuación, cree un `Click` controlador de eventos para el `AddUserToRoleButton` y agregue el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

La mayoría del código de la `Click` controlador de eventos realiza varias comprobaciones de validación. Se asegura de que el visitante proporcionado un nombre de usuario en el `UserNameToAddToRole` cuadro de texto, que el usuario existe en el sistema y que ya no pertenecen al rol seleccionado. Si cualquiera de estas comprobaciones se produce un error, se muestra un mensaje adecuado en `ActionStatus` y se sale del controlador de eventos. Si se superan todas las comprobaciones, el usuario se agrega a la función a través de la `Roles.AddUserToRole` método. A continuación, el cuadro de texto del `Text` propiedad se ha retirado, se actualiza el control GridView y la `ActionStatus` etiqueta muestra un mensaje que indica que el usuario especificado se agregó correctamente a la función seleccionada.

> [!NOTE]
> Para asegurarse de que el usuario especificado no pertenece ya a la función seleccionada, usamos el [ `Roles.IsUserInRole(userName, roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), que devuelve un valor booleano que indica si *nombre de usuario* es un miembro de *roleName*. Usaremos este método nuevo en el <a id="_msoanchor_2"> </a> [siguiente tutorial](role-based-authorization-vb.md) cuando adentrarnos en la autorización basada en roles.


Visite la página a través de un explorador y seleccione el rol de los supervisores de la `RoleList` DropDownList. Pruebe a escribir un nombre de usuario no válido: debe ver un mensaje que explica que el usuario no existe en el sistema.


[![No se puede agregar un usuario inexistente a un rol](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Figura 9**: no se puede agregar un usuario inexistente a una función ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image27.png))


Ahora pruebe a agregar un usuario válido. Continúe y volver a agregar a Tito a la función de los supervisores.


[![Una vez más, Tito es un Supervisor.](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Figura 10**: Tito es, una vez más, un Supervisor.  ([Haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Paso 3: Entre-actualizar el "usuario" y "Role" Interfaces

La `UsersAndRoles.aspx` página ofrece dos interfaces diferentes para administrar usuarios y roles. Actualmente, estas dos interfaces actúan independientemente entre sí por lo que es posible que un cambio realizado en una interfaz no se reflejará inmediatamente en la otra. Por ejemplo, imagine que el visitante a la página selecciona el rol de los supervisores de la `RoleList` DropDownList, que incluye Bruce y Tito como miembros. A continuación, el visitante selecciona Tito desde el `UserList` DropDownList, que comprueba las casillas de los administradores y los supervisores en el `UsersRoleList` repetidor. Si el visitante, a continuación, desactiva el rol de Supervisor de repetidor, Tito se quita de la función de los supervisores, pero esta modificación no se refleja en la interfaz "por rol". GridView seguirá mostrando a Tito como un miembro de la función de los supervisores.

Para solucionar esto se deba actualizar GridView cada vez que un rol está activada o desactivada desde el `UsersRoleList` repetidor. Del mismo modo, es necesario actualizar repetidor cada vez que un usuario se quitan o agregan a un rol de la interfaz "por rol".

Repetidor en la interfaz "por usuario" se actualiza mediante una llamada a la `CheckRolesForSelectedUser` método. La interfaz "por"función puede modificarse en el `RolesUserList` de GridView `RowDeleting` controlador de eventos y el `AddUserToRoleButton` del botón `Click` controlador de eventos. Por lo tanto, es necesario llamar a la `CheckRolesForSelectedUser` método de cada uno de estos métodos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Del mismo modo, se actualiza la GridView en la interfaz "por"función mediante una llamada a la `DisplayUsersBelongingToRole` método y la interfaz "por usuario" se modifica mediante la `RoleCheckBox_CheckChanged` controlador de eventos. Por lo tanto, es necesario llamar a la `DisplayUsersBelongingToRole` método desde este controlador de eventos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Con estos cambios mínimas en el código, el "usuario" y "role" interfaces ahora correctamente entre-update. Para comprobarlo, visite la página a través de un explorador y seleccione Tito y los supervisores de la `UserList` y `RoleList` listas desplegables, respectivamente. Tenga en cuenta que como desactive el rol de los supervisores de Tito de repetidor en la interfaz "por usuario", Tito se quita automáticamente de GridView en la interfaz "por rol". Volver a agregar Tito volver a la función de los supervisores de la interfaz "por"función automáticamente comprobaciones de la casilla de verificación de los supervisores en la interfaz "por usuario".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Paso 4: Personalizar el CreateUserWizard para incluir un paso de "Especificar Roles"

En el <a id="_msoanchor_3"> </a> [ *crear cuentas de usuario* ](../membership/creating-user-accounts-vb.md) tutorial hemos visto cómo usar el control CreateUserWizard Web para proporcionar una interfaz para crear una nueva cuenta de usuario. El control CreateUserWizard puede usarse en una de dos maneras:

- Como un medio para que los visitantes crear su propia cuenta de usuario en el sitio, y
- Como un medio para que los administradores crear nuevas cuentas

En el primer caso de uso, un visitante entra en el sitio y rellena el CreateUserWizard, escriba su información con el fin de registrarse en el sitio. En el segundo caso, un administrador crea una nueva cuenta de otra persona.

Cuando se crea una cuenta de un administrador para cualquier otra persona, puede resultar útil permitir al administrador especificar qué roles de la nueva cuenta de usuario pertenece. En el <a id="_msoanchor_4"> </a> [ *almacenar* *información de usuario adicional* ](../membership/storing-additional-user-information-vb.md) tutorial hemos visto cómo personalizar CreateUserWizard agregando adicionales `WizardSteps`. Echemos un vistazo a cómo agregar un paso adicional a CreateUserWizard con el fin de especificar los nuevos roles del usuario.

Abra la `CreateUserWizardWithRoles.aspx` página y agregue un control CreateUserWizard denominado `RegisterUserWithRoles`. Establecer el control `ContinueDestinationPageUrl` propiedad en "~ / Default.aspx". Dado que la idea es que un administrador va a utilizar este control CreateUserWizard para crear nuevas cuentas de usuario, establecer el control `LoginCreatedUser` propiedad en False. Esto `LoginCreatedUser` propiedad especifica si el visitante se iniciará automáticamente la sesión como el usuario recién creado y el valor predeterminado es True. Se establece en False porque cuando un administrador crea una nueva cuenta deseamos mantener le iniciado sesión con su propia cuenta.

A continuación, seleccione el "Agregar o quitar `WizardSteps`..." opción de etiquetas inteligentes del control CreateUserWizard y agregue un nuevo `WizardStep`, y establece su `ID` a `SpecifyRolesStep`. Mover el `SpecifyRolesStep WizardStep` para que se trata de después del paso de "Inicio de sesión a una nueva cuenta", pero antes del paso "Complete". Establecer el `WizardStep`del `Title` propiedad a "Especificar los Roles," su `StepType` propiedad `Step`y su `AllowReturn` propiedad en False.


[![Agregar el](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Figura 11**: agregar "Especificar Roles" `WizardStep` a CreateUserWizard ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image33.png))


Después de este cambio marcado declarativo del su CreateUserWizard debe ser similar al siguiente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

En la "especificar los Roles" `WizardStep`, agregar un control CheckBoxList denominado `RoleList.` este CheckBoxList mostrará una lista de los roles disponibles, la habilitación de la persona que visita la página comprobar qué roles recién creado que pertenece el usuario.

Seguimos con dos tareas de codificación: primero se debe rellenar el `RoleList` CheckBoxList con los roles en el sistema; en segundo lugar, tenemos que agregar el usuario creado para los roles seleccionados cuando el usuario se mueve desde el paso de "Especificar los Roles" con el paso "Completar". Podemos llevar a cabo la primera tarea de la `Page_Load` controlador de eventos. El siguiente código mediante programación las referencias del `RoleList` casilla de verificación en la primera visita a la página y enlaza los roles en el sistema a él.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

El código anterior debería estar familiarizado. En el <a id="_msoanchor_5"> </a> [ *almacenar* *información de usuario adicional* ](../membership/storing-additional-user-information-vb.md) tutorial se utilizaron dos `FindControl` instrucciones para hacer referencia a un control Web desde dentro de un personalizado `WizardStep`. Y el código que enlace los roles a CheckBoxList tomado de anteriormente en este tutorial.

Para llevar a cabo la segunda tarea de programación, necesitamos saber cuando se ha completado el paso de "Especificar los Roles". Recuerde que CreateUserWizard tiene un `ActiveStepChanged` evento, que se activa cada vez que el visitante navega desde un paso a otro. Aquí podemos determinar si el usuario ha alcanzado el paso "Completar"; Si es así, es necesario agregar el usuario a los roles seleccionados.

Crear un controlador de eventos para el `ActiveStepChanged` eventos y agregue el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Si el usuario ha llegado el paso de "Completed", el controlador de eventos enumera los elementos de la `RoleList` CheckBoxList y el usuario recién creado se asigna a los roles seleccionados.

Visite esta página a través de un explorador. El primer paso en CreateUserWizard es el paso de "Inicio de sesión a una nueva cuenta" estándar, que solicita el nuevo nombre de usuario, contraseña, correo electrónico y otra información clave. Escriba la información para crear un nuevo usuario denominado a Wanda.


[![Crear un nuevo usuario denominado Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Figura 12**: crear un nuevo Wanda con el nombre de usuario ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image36.png))


Haga clic en el botón "Create User". CreateUserWizard llama internamente el `Membership.CreateUser` (método), crear la nueva cuenta de usuario y, a continuación, avanza hasta el paso siguiente, "especifica Roles". A continuación se enumeran los roles de sistema. Active la casilla de los supervisores y haga clic en siguiente.


[![Convertir en miembro de la función de los supervisores Wanda](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Figura 13**: convertir en miembro de la función de los supervisores Wanda ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image39.png))


Haga clic en siguiente provoca una devolución de datos y las actualizaciones de la `ActiveStep` con el paso "Completar". En el `ActiveStepChanged` controlador de eventos, la cuenta de usuario creada recientemente se asigna a la función de los supervisores. Para comprobarlo, volver a la `UsersAndRoles.aspx` página y seleccione los supervisores de la `RoleList` DropDownList. Como se muestra en la figura 14, los supervisores ahora se componen de tres usuarios: Bruce, Tito y Wanda.


[![Bruce, Tito y Wanda son todos los supervisores](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Figura 14**: Bruce, Tito y Wanda son todos los supervisores ([haga clic aquí para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>Resumen

El marco de trabajo de Roles ofrece métodos para recuperar información sobre los roles y los métodos para determinar lo que los usuarios pertenecen a una función especificada de un usuario determinado. Además, hay una serie de métodos para agregar y quitar uno o más usuarios a uno o más roles. En este tutorial se centra en solo dos de estos métodos: `AddUserToRole` y `RemoveUserFromRole`. No hay variantes adicionales que se ha diseñado para agregar varios usuarios a una sola función y asignar varios roles a un único usuario.

Este tutorial también incluye un vistazo a extender el control CreateUserWizard para incluir un `WizardStep` para especificar los roles del usuario recién creado. Un paso de este tipo podría ayudar a un administrador a simplificar el proceso de creación de cuentas de usuario para los nuevos usuarios.

En este punto hemos visto cómo crear y eliminar roles y cómo agregar y quitar usuarios de roles. Pero aún no hemos mirar aplicar la autorización basada en roles. En el <a id="_msoanchor_6"> </a> [siguientes tutorial](role-based-authorization-vb.md) , veremos definir las reglas de autorización de direcciones URL según una función por función, así como la limitar la funcionalidad de nivel de página basado en roles del usuario conectado actualmente.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Resumen de herramienta de administración de sitios Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Examen de ASP. Pertenencia, funciones y perfil de red](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Poner su propia herramienta de administración de sitios Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-and-managing-roles-vb.md)
> [Siguiente](role-based-authorization-vb.md)
