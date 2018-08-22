---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: Asignar Roles a usuarios (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial crearemos dos páginas ASP.NET para ayudar a administrar qué usuarios pertenecen a qué roles. La primera página incluirá facilidades para ver qué...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: 334440f514e94927e428aef8fdba53f0b99e4460
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828390"
---
<a name="assigning-roles-to-users-vb"></a>Asignar Roles a usuarios (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) o [descargar PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> En este tutorial crearemos dos páginas ASP.NET para ayudar a administrar qué usuarios pertenecen a qué roles. La primera página incluirá facilidades para ver qué usuarios pertenecen a un rol determinado, los roles que pertenece un usuario determinado y la capacidad de asignar o quitar un usuario determinado de un rol determinado. En la segunda página aumentaremos el control CreateUserWizard para que incluya un paso para especificar los roles que pertenece el usuario recién creado. Esto es útil en escenarios donde es capaz de crear nuevas cuentas de usuario administrador.


## <a name="introduction"></a>Introducción

El <a id="_msoanchor_1"> </a> [tutorial anterior](creating-and-managing-roles-vb.md) examina el marco de trabajo de Roles y la `SqlRoleProvider`; hemos visto cómo usar el `Roles` clase para crear, recuperar y eliminar roles. Además de crear y eliminar roles, se debe ser capaz de asignar o quitar usuarios de un rol. Lamentablemente, ASP.NET no se distribuye con los controles Web para administrar qué usuarios pertenecen a qué roles. En su lugar, debemos crear nuestros propio páginas ASP.NET para administrar estas asociaciones. La buena noticia es que agregar y quitar usuarios a roles es bastante fácil. La `Roles` clase contiene una serie de métodos para agregar uno o varios usuarios a uno o varios roles.

En este tutorial crearemos dos páginas ASP.NET para ayudar a administrar qué usuarios pertenecen a qué roles. La primera página incluirá facilidades para ver qué usuarios pertenecen a un rol determinado, los roles que pertenece un usuario determinado y la capacidad de asignar o quitar un usuario determinado de un rol determinado. En la segunda página aumentaremos el control CreateUserWizard para que incluya un paso para especificar los roles que pertenece el usuario recién creado. Esto es útil en escenarios donde es capaz de crear nuevas cuentas de usuario administrador.

Comencemos.

## <a name="listing-what-users-belong-to-what-roles"></a>Lista de qué usuarios pertenecen a qué Roles

La primera regla de negocio para este tutorial es crear una página web desde el que se pueden asignar usuarios a roles. Antes de que se refieren a nosotros mismos con la forma de asignar a usuarios a roles, vamos a concentrarse en primer lugar cómo determinar qué usuarios pertenecen a qué roles. Hay dos maneras de mostrar esta información: "rol" o ""usuario. Podríamos permitir que el visitante seleccionar un rol y, a continuación, mostrarlos todos los usuarios que pertenecen a la función (la presentación "por el rol") o se puede pedir el visitante para seleccionar un usuario y, a continuación, mostrarles los roles asignados a ese usuario (la presentación "por usuario").

La vista "por"función es útil en circunstancias donde el visitante desea conocer el conjunto de usuarios que pertenecen a una función concreta; la vista "por usuario" es ideal cuando el visitante debe saber los roles de un usuario determinado. Vamos a que nuestra página incluya tanto "role" y "usuario" interfaces.

Comenzamos con la creación de la interfaz "por usuario". Esta interfaz constará de una lista desplegable y una lista de casillas de verificación. La lista desplegable se rellenará con el conjunto de usuarios en el sistema; las casillas de verificación enumerará los roles. Al seleccionar un usuario de la lista desplegable comprobará que esos roles que pertenece el usuario. La persona que visita la página puede, a continuación, active o desactive las casillas de verificación para agregar o quitar el usuario seleccionado de los roles correspondientes.

> [!NOTE]
> Uso de una lista desplegable lista a las cuentas de usuario no es una opción ideal para sitios Web que puede haber cientos de cuentas de usuario. Una lista desplegable está diseñada para permitir que un usuario seleccionar un elemento de una lista relativamente corta de opciones. Rápidamente resulta difícil de manejar a medida que crece el número de elementos de lista. Si está creando un sitio Web que tendrá potencialmente grandes números de cuentas de usuario, es aconsejable considere el uso de una interfaz de usuario alternativa, como un control GridView paginable o una interfaz filtrable que se enumera solicita el visitante para elegir una letra y, a continuación, solo Muestra los usuarios cuyo nombre de usuario comienza por la letra seleccionada.


## <a name="step-1-building-the-by-user-user-interface"></a>Paso 1: Creación de la interfaz de usuario "Por usuario"

Abra el `UsersAndRoles.aspx` página. En la parte superior de la página, agregue un control de etiqueta Web denominado `ActionStatus` y borre su `Text` propiedad. Esta etiqueta se usará para proporcionar comentarios sobre las acciones realizadas, mostrar mensajes como, "Tito de usuario se ha agregado a la función Administradores" o "Usuario Jisun se quitó de la función de los supervisores." Para hacer que estas destacan los mensajes, Establece la etiqueta `CssClass` propiedad a "Importante".

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

A continuación, agregue la siguiente definición de clase CSS para el `Styles.css` hoja de estilos:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Esta definición CSS indica al explorador para mostrar la etiqueta con una fuente grande, rojo. Figura 1 muestra este efecto mediante el Diseñador de Visual Studio.


[![Propiedad de la etiqueta CssClass da como resultado una fuente grande, roja](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Figura 1**: la etiqueta `CssClass` propiedad da como resultado un gran tamaño, fuente de color rojo ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image3.png))


A continuación, agregue un DropDownList a la página, establezca su `ID` propiedad `UserList`y establezca su `AutoPostBack` propiedad en True. Usaremos esta DropDownList para enumerar todos los usuarios en el sistema. Este DropDownList se enlazará a una colección de objetos de MembershipUser. Porque queremos DropDownList para mostrar la propiedad UserName del objeto MembershipUser (y usarlo como el valor de los elementos de lista), establezca la DropDownList `DataTextField` y `DataValueField` propiedades en "UserName".

Debajo de la DropDownList, agregue un control Repeater denominado `UsersRoleList`. Este control Repeater enumera todos los roles en el sistema como una serie de casillas de verificación. Definir el repetidor `ItemTemplate` mediante el siguiente marcado declarativo:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

El `ItemTemplate` marcado incluye un control de casilla de verificación Web único denominado `RoleCheckBox`. La casilla de verificación `AutoPostBack` propiedad está establecida en True y el `Text` propiedad está enlazada a `Container.DataItem`. La razón por la sintaxis de enlace de datos es simplemente `Container.DataItem` es porque el marco de trabajo Roles devuelve la lista de nombres de función como una matriz de cadenas, y es esta matriz de cadenas que realizaremos el enlace para el control Repeater. Una descripción detallada de por qué esta sintaxis se utiliza para mostrar el contenido de una matriz que se enlaza a un control Web de datos queda fuera del ámbito de este tutorial. Para obtener más información sobre este asunto, consulte [enlazar una matriz de escalar a un Control de datos Web](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

En este punto marcado declarativo de la interfaz de "por usuario" debe ser similar al siguiente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Ahora estamos preparados escribir el código para enlazar el conjunto de cuentas de usuario al control DropDownList y el conjunto de roles para el control Repeater. En la clase de código subyacente de la página, agregue un método denominado `BindUsersToUserList` y otro denominado `BindRolesList`, utilizando el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

El `BindUsersToUserList` método recupera todas las cuentas de usuario en el sistema a través de la [ `Membership.GetAllUsers` método](https://msdn.microsoft.com/library/dy8swhya.aspx). Esto devuelve un [ `MembershipUserCollection` objeto](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), que es una colección de [ `MembershipUser` instancias](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Esta colección se enlaza a la `UserList` DropDownList. El `MembershipUser` instancias que utilizan la colección contiene una serie de propiedades, como `UserName`, `Email`, `CreationDate`, y `IsOnline`. Con el fin de indicar la DropDownList para mostrar el valor de la `UserName` propiedad, asegúrese de que el `UserList` de DropDownList `DataTextField` y `DataValueField` propiedades se han establecido en "UserName".

> [!NOTE]
> El `Membership.GetAllUsers` método tiene dos sobrecargas: uno que no acepta ningún parámetro de entrada y devuelve todos los usuarios y otro que acepta valores enteros para el índice de la página y el tamaño de página y devuelve solo el subconjunto especificado de los usuarios. Cuando existen grandes cantidades de cuentas de usuario que se muestra en un elemento de interfaz de usuario paginable, la segunda sobrecarga se puede usar para la página de forma más eficaz a través de los usuarios debido a que devuelve solo el subconjunto preciso de las cuentas de usuario en lugar de todos ellos.


El `BindRolesToList` método comienza mediante una llamada a la `Roles` la clase [ `GetAllRoles` método](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), que devuelve una matriz de cadenas que contiene los roles en el sistema. Esta matriz de cadenas, a continuación, se enlaza a Repeater.

Por último, es necesario llamar a estos dos métodos cuando se carga la página por primera vez. Agregue el código siguiente al controlador de eventos `Page_Load`:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Con este código en su lugar, tómese un momento para visitar la página a través de un explorador; la pantalla debe ser similar a la figura 2. Todas las cuentas de usuario se rellenan en la lista desplegable y, por debajo de ésta, cada rol aparece como una casilla de verificación. Dado que establecemos la `AutoPostBack` propiedades de la DropDownList y casillas de verificación en True, cambiar el usuario seleccionado o activando o desactivando una función produce un postback. Sin embargo, se realiza ninguna acción porque todavía tenemos que escribir código para controlar estas acciones. Trataremos estas tareas en las dos secciones siguientes.


[![La página muestra los usuarios y Roles](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Figura 2**: la página muestra los usuarios y Roles ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Comprobación de los Roles seleccionado pertenece el usuario

Cuando se carga la página por primera vez, o cada vez que el visitante selecciona un nuevo usuario en la lista desplegable, deberá actualizar el `UsersRoleList`de casillas de verificación para que se activa una casilla de rol determinado solo si el usuario seleccionado pertenece a ese rol. Para ello, cree un método denominado `CheckRolesForSelectedUser` con el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

El código anterior se inicia mediante la determinación de quién es el usuario seleccionado. A continuación, se utiliza la clase Roles [ `GetRolesForUser(userName)` método](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) para devolver el conjunto del usuario especificado de funciones como una matriz de cadenas. A continuación, se enumeran los elementos de Repeater y cada elemento `RoleCheckBox` casilla mediante programación se hace referencia. Se activa la casilla solo si está dentro de la función se corresponde con el `selectedUsersRoles` matriz de cadenas.

> [!NOTE]
> El `Linq.Enumerable.Contains(Of String)(...)` sintaxis no se compilará si usa la versión 2.0 de ASP.NET. El `Contains(Of String)` método forma parte de la [biblioteca LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), que es una novedad de ASP.NET 3.5. Si aún utiliza la versión 2.0 de ASP.NET, utilice el [ `Array.IndexOf(Of String)` método](https://msdn.microsoft.com/library/eha9t187.aspx) en su lugar.


El `CheckRolesForSelectedUser` método debe llamarse en dos casos: cuando se carga la página por primera vez y siempre que el `UserList` se cambia el índice seleccionado de DropDownList. Por lo tanto, llamar a este método desde el `Page_Load` controlador de eventos (después de las llamadas a `BindUsersToUserList` y `BindRolesToList`). Además, cree un controlador de eventos para la DropDownList `SelectedIndexChanged` evento y llamar a este método desde allí.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Con este código en su lugar, puede probar la página a través del explorador. Sin embargo, dado que la `UsersAndRoles.aspx` página actualmente carece de la capacidad para asignar usuarios a roles, ningún usuario tiene roles. Vamos a crear la interfaz para asignar usuarios a roles en un momento, por lo que puede tomar una palabra que este código funciona y compruebe que lo hace más tarde, o puede agregar manualmente usuarios a roles mediante la inserción de registros en el `aspnet_UsersInRoles` tabla con el fin de probar este functi onality ahora.

### <a name="assigning-and-removing-users-from-roles"></a>Asignar y quitar usuarios de Roles

Cuando el visitante comprueba o desactiva una casilla de verificación en la `UsersRoleList` Repeater que necesitamos para agregar o quitar el usuario seleccionado de la función correspondiente. La casilla de verificación `AutoPostBack` propiedad actualmente está establecida en True, lo que hace que una devolución de datos cada vez que una casilla en el control Repeater está activado o desactivado. En resumen, es necesario crear un controlador de eventos de la casilla `CheckChanged` eventos. Puesto que la casilla de verificación está en un control Repeater, tenemos que agregar manualmente la mecánica de controlador de eventos. Empiece por agregar el controlador de eventos a la clase de código subyacente, como un `Protected` método, de este modo:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Se devolverá para escribir el código para este controlador de eventos en un momento. Pero primero vamos a finalizar la estructura de control de eventos. En la casilla del control Repeater del `ItemTemplate`, agregar `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Esta sintaxis se conecta el `RoleCheckBox_CheckChanged` controlador de eventos para el `RoleCheckBox`del `CheckedChanged` eventos.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

Nuestra última tarea consiste en completar la `RoleCheckBox_CheckChanged` controlador de eventos. Debemos comenzar haciendo referencia al control CheckBox que provocó el evento porque esta instancia de la casilla de verificación nos indica qué rol se ha comprobado o a través de su `Text` y `Checked` propiedades. Con esta información junto con el nombre de usuario del usuario seleccionado, se agregar o quitar el usuario de la función a través de la `Roles` la clase [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) o [ `RemoveUserFromRole` método](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

El código anterior se inicia haciendo mediante programación la casilla de verificación que generó el evento, que está disponible a través de la `sender` parámetro de entrada. Si la casilla de verificación está activada, el usuario seleccionado se agrega al rol especificado, en caso contrario, se quitan de la función. En cualquier caso, el `ActionStatus` etiqueta muestra un mensaje de resumen de la acción que acaba de realizar.

Dedique un momento para probar esta página a través de un explorador. Seleccione usuario Tito y, a continuación, agregue a Tito a administradores y los supervisores de roles.


[![Se ha agregado Tito a los administradores y los supervisores Roles](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Figura 3**: Tito se ha agregado a los administradores y Roles de los supervisores ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image9.png))


A continuación, seleccione el usuario Bruce en la lista desplegable. Hay una devolución de datos y las casillas de verificación de Repeater se actualizan a través de la `CheckRolesForSelectedUser`. Puesto que Bruce aún no pertenecen a ningún rol, se desactivan las dos casillas. A continuación, agregue a Bruce a la función de los supervisores.


[![Bruce se ha agregado a la función de los supervisores](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Figura 4**: Bruce se ha agregado a la función de los supervisores ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image12.png))


Para comprobar la funcionalidad de la `CheckRolesForSelectedUser` método, seleccione un usuario que no sean Tito o Bruce. Tenga en cuenta cómo se automáticamente desactivan las casillas de verificación, que indica que no pertenecen a ningún rol. Volver a Tito. Se deben comprobar tanto los administradores y los supervisores de casillas de verificación.

## <a name="step-2-building-the-by-roles-user-interface"></a>Paso 2: Creación de la interfaz de usuario "Por Roles"

En este momento se ha completado la interfaz "by users" y está listos para comenzar a abordar la interfaz "por roles". La interfaz "por roles" pide al usuario que seleccione un rol en una lista desplegable y, a continuación, muestra el conjunto de usuarios que pertenecen a ese rol en un control GridView.

Agregue otro control DropDownList para la `UsersAndRoles.aspx page`. Coloca este uno debajo del control Repeater, asígnele el nombre `RoleList`y establezca su `AutoPostBack` propiedad en True. Por debajo de ésta, agregue un control GridView y asígnele el nombre `RolesUserList`. Este control GridView mostrará los usuarios que pertenecen al rol seleccionado. Establezca la GridView `AutoGenerateColumns` propiedad en False, agregue un TemplateField a la cuadrícula `Columns` colección y establezca su `HeaderText` propiedad a "Usuarios". Definir el TemplateField `ItemTemplate` para que se muestre el valor de la expresión de enlace de datos `Container.DataItem` en el `Text` propiedad de una etiqueta denominada `UserNameLabel`.

Después de agregar y configurar el control GridView, el marcado declarativo de la interfaz de "por el rol" debe ser similar al siguiente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

Se debe rellenar el `RoleList` DropDownList con el conjunto de roles en el sistema. Para lograr esto, actualice el `BindRolesToList` método así que enlaza la matriz de cadena devuelta por la `Roles.GetAllRoles` método a la `RolesList` DropDownList (así como el `UsersRoleList` Repeater).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Las dos últimas líneas en el `BindRolesToList` método se han agregado para enlazar el conjunto de roles para el `RoleList` DropDownList (control). Figura 5 muestra el resultado final cuando se ve mediante un explorador: una lista desplegable se rellena con los roles del sistema.


[![Los Roles se muestran en RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Figura 5**: los Roles se muestran en el `RoleList` DropDownList ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Mostrar los usuarios que pertenecen al rol seleccionado

Cuando se carga la página por primera vez o cuando se selecciona un nuevo rol de la `RoleList` DropDownList, es necesario mostrar la lista de usuarios que pertenecen a ese rol en el control GridView. Cree un método denominado `DisplayUsersBelongingToRole` con el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Este método comienza mediante la obtención de la función seleccionada desde la `RoleList` DropDownList. A continuación, usa el [ `Roles.GetUsersInRole(roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) para recuperar una matriz de cadenas de los nombres de usuario de los usuarios que pertenecen a ese rol. Esta matriz se enlaza a la `RolesUserList` GridView.

Este método debe llamarse en dos casos: cuando se carga la página por primera vez y cuando el rol seleccionado en el `RoleList` DropDownList cambios. Por lo tanto, actualizar la `Page_Load` controlador de eventos para que este método se invoca después de llamar a `CheckRolesForSelectedUser`. A continuación, cree un controlador de eventos para el `RoleList`del `SelectedIndexChanged` eventos y llamar a este método a partir de ahí, demasiado.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Con este código en su lugar, el `RolesUserList` GridView debe mostrar los usuarios que pertenecen a la función seleccionada. Como se muestra en la figura 6, el rol de los supervisores consta de dos miembros: Bruce y Tito.


[![El control GridView enumera los usuarios que pertenecen al rol seleccionado](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Figura 6**: The GridView se enumeran los usuarios que pertenecen al rol seleccionado ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Quitar usuarios del rol seleccionado

Vamos a aumentar la `RolesUserList` botones GridView para que incluya una columna de "quitar". Al hacer clic en el botón "Quitar" para un usuario determinado se quitarán de ese rol.

Empiece agregando un campo de botón de eliminación en el control GridView. Que este campo aparezca como de la izquierda de archivado más y cambiar su `DeleteText` propiedad de "Delete" (predeterminado) a "Quitar".


[![Agregar el](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Figura 7**: agregar el botón "Quitar" en el control GridView ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image21.png))


Cuando se hace clic en el botón "Quitar" que habrá trastornos una devolución de datos y la GridView `RowDeleting` provoca el evento. Es necesario crear un controlador de eventos para este evento y escribir código que se quita al usuario el rol seleccionado. Cree el controlador de eventos y, a continuación, agregue el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

El código empieza por determinar el nombre del rol seleccionado. A continuación, mediante programación las referencias del `UserNameLabel` control de la fila cuyo botón "Quitar" se hizo clic con el fin de determinar el nombre de usuario del usuario para quitar. El usuario, a continuación, se quita de la función mediante una llamada a la `Roles.RemoveUserFromRole` método. El `RolesUserList` GridView, a continuación, se actualiza y se muestra un mensaje a través de la `ActionStatus` control de etiqueta.

> [!NOTE]
> El botón "Quitar" no es necesario a algún tipo de confirmación del usuario antes de quitar el usuario de la función. Le invito a agregar cierto nivel de confirmación del usuario. Una de las maneras más fáciles para confirmar una acción es a través de un cuadro de diálogo de confirmación del lado cliente. Para obtener más información sobre esta técnica, consulte [adición del lado cliente confirmación cuando se elimina](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Figura 8 muestra la página después de quitar del grupo los supervisores de usuario Tito.


[![Lamentablemente, Tito ya No es un Supervisor](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Figura 8**: Lamentablemente, Tito ya No es un Supervisor ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Adición de nuevos usuarios al rol seleccionado

Junto con quitando usuarios de la función seleccionada, el visitante a esta página también debe ser capaz de agregar un usuario al rol seleccionado. La interfaz de procedimiento para agregar un usuario al rol seleccionado depende del número de cuentas de usuario que espera tener. Si su sitio Web hospedará pocas cuentas de usuario docenas o menos, podría utilizar DropDownList aquí. Si puede haber miles de cuentas de usuario, desearía incluyen una interfaz de usuario que permite que el visitante para desplazarse a través de las cuentas, una cuenta determinada, buscar o filtrar las cuentas de usuario de algún otro modo.

Para esta página Vamos a usar una interfaz muy simple que funciona independientemente del número de cuentas de usuario en el sistema. Es decir, usaremos un cuadro de texto, preguntar el visitante para escribir en el nombre de usuario del usuario que desea agregar al rol seleccionado. Si no existe ningún usuario con ese nombre, o si el usuario ya es miembro del rol, se mostrará un mensaje en `ActionStatus` etiqueta. Pero si el usuario existe y no es un miembro del rol, deberá agregarlos al rol y actualizar la cuadrícula.

Agregue un cuadro de texto y un botón debajo del control GridView. Establezca el cuadro de texto `ID` a `UserNameToAddToRole` y establezca el botón `ID` y `Text` propiedades a `AddUserToRoleButton` y "Agregar al rol de usuario", respectivamente.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

A continuación, cree un `Click` controlador de eventos para el `AddUserToRoleButton` y agregue el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

La mayoría del código en el `Click` controlador de eventos realiza varias comprobaciones de validación. Se asegura de que el visitante especificado un nombre de usuario en el `UserNameToAddToRole` TextBox, que el usuario existe en el sistema y que ya no pertenecen al rol seleccionado. Si cualquiera de estas comprobaciones se produce un error, se muestra un mensaje adecuado en `ActionStatus` y se sale del controlador de eventos. Si se superan todas las comprobaciones, el usuario se agrega a la función a través de la `Roles.AddUserToRole` método. A continuación, el cuadro de texto del `Text` propiedad se ha retirado, se actualiza el control GridView y el `ActionStatus` etiqueta muestra un mensaje que indica que el usuario especificado se agregó correctamente a la función seleccionada.

> [!NOTE]
> Para asegurarse de que el usuario especificado no pertenece al rol seleccionado, usamos el [ `Roles.IsUserInRole(userName, roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), que devuelve un valor booleano que indica si *userName* es un miembro de *roleName*. Usamos este método nuevo en el <a id="_msoanchor_2"> </a> [siguiente tutorial](role-based-authorization-vb.md) cuando nos centramos en la autorización basada en roles.


Visite la página a través de un explorador y seleccione el rol de los supervisores de la `RoleList` DropDownList. Pruebe a escribir un nombre de usuario no válido, verá un mensaje que explica que el usuario no existe en el sistema.


[![No se puede agregar un usuario inexistente a un rol](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Figura 9**: no se puede agregar un usuario inexistente a una función ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image27.png))


Ahora intente agregar un usuario válido. Siga adelante y volver a agregar a Tito a la función de los supervisores.


[![¡Una vez más, Tito es un Supervisor!](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Figura 10**: Tito es, una vez más, un Supervisor!  ([Haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Paso 3: Entre-actualizar el "Por usuario" y "rol" Interfaces

El `UsersAndRoles.aspx` página ofrece dos interfaces distintas para administrar usuarios y roles. Actualmente, estas dos interfaces actúan independientemente entre sí por lo que es posible que un cambio realizado en una interfaz no se reflejará inmediatamente en el otro. Por ejemplo, imagine que el visitante de la página, selecciona el rol de los supervisores de la `RoleList` DropDownList, donde se muestra Bruce y Tito como miembros. A continuación, el visitante selecciona Tito desde el `UserList` DropDownList, que comprueba las casillas de los administradores y los supervisores en el `UsersRoleList` Repeater. Si el visitante, a continuación, desactiva el rol de Supervisor desde el control Repeater, Tito se quita de la función de los supervisores, pero esta modificación no se refleja en la interfaz "por el rol". El control GridView seguirá mostrando a Tito como un miembro del rol de los supervisores.

Para corregir esto se deba actualizar el control GridView cada vez que un rol está activado o desactivado desde el `UsersRoleList` Repeater. Del mismo modo, se deba actualizar el control Repeater cada vez que un usuario se quita o se agrega a un rol de la interfaz "por el rol".

El control Repeater en la interfaz "por usuario" se actualiza mediante una llamada a la `CheckRolesForSelectedUser` método. Se puede modificar la interfaz "por el rol" en el `RolesUserList` de GridView `RowDeleting` controlador de eventos y el `AddUserToRoleButton` del botón `Click` controlador de eventos. Por lo tanto, es necesario llamar a la `CheckRolesForSelectedUser` método de cada uno de estos métodos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Del mismo modo, se actualiza el control GridView en la interfaz "por el rol" mediante una llamada a la `DisplayUsersBelongingToRole` se modifica el método y la interfaz "por usuario" a través de la `RoleCheckBox_CheckChanged` controlador de eventos. Por lo tanto, es necesario llamar a la `DisplayUsersBelongingToRole` método desde este controlador de eventos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Con estos cambios de código secundario, el "por usuario" y "rol" interfaces ahora correctamente entre la actualización. Para comprobarlo, visite la página a través de un explorador y seleccione Tito y los supervisores de la `UserList` y `RoleList` DropDownLists, respectivamente. Tenga en cuenta que como desactive el rol de los supervisores de Tito desde el control Repeater en la interfaz "por usuario", Tito se quita automáticamente del control GridView en la interfaz "por el rol". Volver a agregar Tito volver a la función de los supervisores de la interfaz "por el rol" automáticamente comprobaciones de la casilla de verificación de los supervisores en la interfaz "por usuario".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Paso 4: Personalizar el control CreateUserWizard para incluir un paso "Especificar Roles"

En el <a id="_msoanchor_3"> </a> [ *crear cuentas de usuario* ](../membership/creating-user-accounts-vb.md) tutorial hemos visto cómo usar el control CreateUserWizard Web para proporcionar una interfaz para crear una nueva cuenta de usuario. El control CreateUserWizard puede usarse en uno de dos maneras:

- Como un medio para que los visitantes crear su propia cuenta de usuario en el sitio, y
- Como un medio para que los administradores crear nuevas cuentas

En el primer caso de uso, un visitante entra en el sitio y rellena el control CreateUserWizard, especificación de su información con el fin de registrarse en el sitio. En el segundo caso, un administrador crea una nueva cuenta de otra persona.

Cuando se crea una cuenta de un administrador para cualquier otra persona, puede resultar útil permitir que el administrador especificar los roles que pertenece la nueva cuenta de usuario. En el <a id="_msoanchor_4"> </a> [ *almacenar* *información de usuario adicional* ](../membership/storing-additional-user-information-vb.md) tutorial hemos visto cómo personalizar el control CreateUserWizard agregando adicionales `WizardSteps`. Echemos un vistazo a cómo agregar un paso adicional para el control CreateUserWizard con el fin de especificar los nuevos roles del usuario.

Abra el `CreateUserWizardWithRoles.aspx` página y agregue un control CreateUserWizard denominado `RegisterUserWithRoles`. Establecer el control `ContinueDestinationPageUrl` propiedad en "~ / Default.aspx". Dado que la idea aquí es que un administrador va a utilizar este control CreateUserWizard para crear nuevas cuentas de usuario, establezca el control `LoginCreatedUser` propiedad en False. Esto `LoginCreatedUser` propiedad especifica si el visitante inicia automáticamente la sesión como el usuario que acaba de crear, y el valor predeterminado es True. Se establece en False porque cuando un administrador crea una nueva cuenta van a conservar en contacto con él iniciado sesión con su propia cuenta.

A continuación, seleccione el "Agregar o quitar `WizardSteps`..." opción de etiqueta inteligente del control CreateUserWizard y agregue un nuevo `WizardStep`, estableciendo su `ID` a `SpecifyRolesStep`. Mover el `SpecifyRolesStep WizardStep` para que se trata de después del paso de "Inicio de sesión de nuevo tu cuenta", pero antes del paso "Completado". Establecer el `WizardStep`del `Title` propiedad a "Especificar Roles", su `StepType` propiedad `Step`y su `AllowReturn` propiedad en False.


[![Agregar el](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Figura 11**: agregue "Especificar Roles" `WizardStep` a CreateUserWizard ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image33.png))


Después de este cambio marcado declarativo de su control CreateUserWizard debe ser similar al siguiente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

"Especificar roles" `WizardStep`, agregue un control CheckBoxList denominado `RoleList.` CheckBoxList este enumerará los roles disponibles, la habilitación de la persona que visite la página comprobar qué roles recién creado que pertenece el usuario.

Seguimos teniendo las tareas de codificación de dos: primero se debe rellenar el `RoleList` CheckBoxList con los roles en el sistema; en segundo lugar, tenemos que agregar el usuario creado para los roles seleccionados, cuando el usuario se mueve desde el paso "Especificar Roles" con el paso "Complete". Podemos llevar a cabo la primera tarea en el `Page_Load` controlador de eventos. El siguiente código mediante programación las referencias del `RoleList` casilla de verificación en la primera visita a la página y enlaza los roles en el sistema en él.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

El código anterior le resultará familiar. En el <a id="_msoanchor_5"> </a> [ *almacenar* *información de usuario adicional* ](../membership/storing-additional-user-information-vb.md) tutorial hemos usado dos `FindControl` instrucciones para hacer referencia a un control Web desde dentro de un personalizado `WizardStep`. Y el código que enlaza los roles a CheckBoxList se haya obtenido anteriormente en este tutorial.

Para llevar a cabo la segunda tarea de programación, necesitamos saber cuándo se ha completado el paso "Especificar Roles". Recuerde que el control CreateUserWizard tiene un `ActiveStepChanged` evento, que se desencadena cada vez que el visitante navega desde un paso a otro. Aquí podemos determinar si el usuario ha alcanzado el paso "Completar"; Si es así, necesitamos agregar el usuario a los roles seleccionados.

Crear un controlador de eventos para el `ActiveStepChanged` eventos y agregue el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Si el usuario simplemente ha alcanzado el paso "Completado", el controlador de eventos enumera los elementos de la `RoleList` CheckBoxList y el usuario acaba de crear se asigna a los roles seleccionados.

Visite esta página a través de un explorador. El primer paso en el control CreateUserWizard es el paso de "Inicio de sesión de nuevo tu cuenta" estándar, que solicita el nuevo nombre de usuario, contraseña, correo electrónico y otra información clave. Escriba la información para crear un nuevo usuario denominado a Wanda.


[![Crear un nuevo usuario denominado Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Figura 12**: crear un nuevo Wanda con nombre de usuario ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image36.png))


Haga clic en el botón "Crear usuario". El control CreateUserWizard llama internamente a la `Membership.CreateUser` (método), crear la nueva cuenta de usuario y, a continuación, continúa con el paso siguiente, "especifica Roles". Aquí se enumeran los roles de sistema. Active la casilla de verificación de los supervisores y haga clic en siguiente.


[![Convertir en miembro de la función de los supervisores Wanda](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Figura 13**: convertir en miembro de la función de los supervisores Wanda ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image39.png))


Haga clic en siguiente hace que una devolución de datos y las actualizaciones de la `ActiveStep` con el paso "Complete". En el `ActiveStepChanged` controlador de eventos, la cuenta de usuario recientemente creado se asigna a la función de los supervisores. Para comprobarlo, volver a la `UsersAndRoles.aspx` página y seleccione los supervisores de la `RoleList` DropDownList. Como se muestra en la figura 14, los supervisores ahora se componen de tres usuarios: Bruce Tito y Wanda.


[![Bruce, Tito y Wanda son supervisores de todo](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Figura 14**: Bruce, Tito y Wanda son todos los supervisores ([haga clic aquí para ver imagen en tamaño completo](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>Resumen

El marco de Roles ofrece métodos para recuperar información sobre los roles y los métodos para determinar qué usuarios pertenecen a una función especificada de un usuario determinado. Además, existen varios métodos para agregar y quitar uno o varios usuarios a uno o varios roles. En este tutorial se centra en sólo dos de estos métodos: `AddUserToRole` y `RemoveUserFromRole`. Hay variantes adicionales diseñadas para agregar varios usuarios a una sola función y asignar varios roles a un único usuario.

En este tutorial también incluye un vistazo al extender el control CreateUserWizard para incluir un `WizardStep` para especificar los roles del usuario recién creado. Un paso de este tipo podría ayudar a un administrador a agilizar el proceso de creación de cuentas de usuario para los nuevos usuarios.

En este punto hemos visto cómo crear y eliminar roles y cómo agregar y quitar usuarios de roles. Pero aún tenemos que mirar al aplicar la autorización basada en roles. En el <a id="_msoanchor_6"> </a> [siguiente tutorial](role-based-authorization-vb.md) veremos definir reglas de autorización de dirección URL según el rol por rol, así como la forma limitar la funcionalidad de nivel de página basado en roles del usuario con sesión iniciada actualmente.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Información general de ASP.NET Web Site Administration Tool](https://msdn.microsoft.com/library/ms228053.aspx)
- [Examen de ASP. Perfil, Roles y pertenencia a la red](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Implementar su propia herramienta de administración del sitio Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-and-managing-roles-vb.md)
> [Siguiente](role-based-authorization-vb.md)
