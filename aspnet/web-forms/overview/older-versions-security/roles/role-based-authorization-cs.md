---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: Autorización basada en roles (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial comienza con un vistazo a cómo el marco de trabajo de Roles asocia los roles de un usuario con su contexto de seguridad. A continuación, se examina cómo aplicar la dirección URL basada en roles...
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 243a386bf682d63e3095a4e90ca0a0f08d24b503
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839952"
---
<a name="role-based-authorization-c"></a>Autorización basada en roles (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) o [descargar PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> Este tutorial comienza con un vistazo a cómo el marco de trabajo de Roles asocia los roles de un usuario con su contexto de seguridad. A continuación, se examina cómo aplicar reglas de autorización de dirección URL basada en roles. A continuación, veremos un medio declarativo y programático para modificar los datos mostrados y la funcionalidad que ofrece una página ASP.NET.


## <a name="introduction"></a>Introducción

En el <a id="_msoanchor_1"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-cs.md) tutorial vimos cómo usar la autorización de dirección URL para especificar qué usuarios pueden visitar un determinado conjunto de páginas. Con un poco de marcado en `Web.config`, nos podríamos indicar a ASP.NET para permitir que solo los usuarios autenticados visitar una página. O nos podríamos dictan que solo los usuarios Tito y Bob se pueden o indican que se permiten todos los usuarios autenticados, excepto de Sam.

Además de autorización de URL, también hemos analizado declarativas y programáticos técnicas para controlar los datos mostrados y la funcionalidad que ofrece una página basada en el usuario que visita. En concreto, hemos creado una página que aparece el contenido del directorio actual. Cualquier persona puede visitar esta página, pero solo los usuarios autenticados podrían ver el contenido de los archivos y Tito sólo puede eliminar los archivos.

Aplicar reglas de autorización según el usuario por el usuario puede convertirse en una pesadilla de contabilidad. Un enfoque más fácil de mantener es usar la autorización basada en roles. La buena noticia es que las herramientas a nuestra disposición para aplicar las reglas de autorización funcionan igual de bien con funciones como lo hacen las cuentas de usuario. Las reglas de autorización de dirección URL pueden especificar los roles en lugar de los usuarios. El control LoginView, que representa una salida diferente para los usuarios autenticados y anónimos, puede configurarse para mostrar contenido diferente en función de los roles del usuario que ha iniciado sesión. Y la API de Roles incluye métodos para determinar los roles del usuario que ha iniciado sesión.

Este tutorial comienza con un vistazo a cómo el marco de trabajo de Roles asocia los roles de un usuario con su contexto de seguridad. A continuación, se examina cómo aplicar reglas de autorización de dirección URL basada en roles. A continuación, veremos un medio declarativo y programático para modificar los datos mostrados y la funcionalidad que ofrece una página ASP.NET. Comencemos.

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Comprender cómo Roles están asociados con contexto de seguridad de un usuario

Cada vez que una solicitud entra en la canalización de ASP.NET está asociado con un contexto de seguridad, que incluye información que identifica el solicitante. Cuando se usa la autenticación de formularios, se usa un vale de autenticación como un token de identidad. Como se explicó en la <a id="_msoanchor_2"> </a> [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-cs.md) y <a id="_msoanchor_3"> </a> [ *formularios Configuración de autenticación y temas avanzados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) tutoriales, la `FormsAuthenticationModule` es responsable de determinar la identidad del solicitante, lo que sucede durante el [ `AuthenticateRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Si se encuentra un vale de autenticación válido y que no hayan caducado, el `FormsAuthenticationModule` descodifica para determinar la identidad del solicitante. Crea un nuevo `GenericPrincipal` objeto y se asigna esta opción para la `HttpContext.User` objeto. El propósito de una entidad de seguridad, como `GenericPrincipal`, consiste en identificar el nombre del usuario autenticado y lo que pertenecen a las funciones. Con este fin es evidente por el hecho de que todos los objetos de entidad de seguridad tienen un `Identity` propiedad y un `IsInRole(roleName)` método. El `FormsAuthenticationModule`, sin embargo, no está interesado en la grabación de información de roles y los `GenericPrincipal` crea el objeto no especifica ningún rol.

Si el marco de trabajo de Roles está habilitada, el [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) módulo HTTP después de que los pasos de la `FormsAuthenticationModule` e identifica los roles del usuario autenticado durante el [ `PostAuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que se activa tras la `AuthenticateRequest` eventos. Si la solicitud proviene de un usuario autenticado, el `RoleManagerModule` sobrescribe el `GenericPrincipal` objeto creado por el `FormsAuthenticationModule` y lo reemplaza con un [ `RolePrincipal` objeto](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). La `RolePrincipal` clase usa la API de Roles para determinar los roles que pertenece el usuario.

Figura 1 se muestra el flujo de trabajo de canalización ASP.NET cuando se usa la autenticación de formularios y el marco de trabajo de Roles. El `FormsAuthenticationModule` se ejecuta en primer lugar, identifica al usuario a través de su vale de autenticación y crea un nuevo `GenericPrincipal` objeto. A continuación, el `RoleManagerModule` interviene y sobrescribe el `GenericPrincipal` objeto con un `RolePrincipal` objeto.

Si un usuario anónimo visita el sitio, ni el `FormsAuthenticationModule` ni `RoleManagerModule` crea un objeto de entidad.


[![Los eventos de canalización de ASP.NET para un usuario autenticado cuando se usa la autenticación de formularios y el marco de trabajo de Roles](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Figura 1**: los eventos de canalización de ASP.NET para un autenticado usuario al utilizar autenticación de formularios y el marco de trabajo de Roles ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Almacenamiento en caché información de funciones en una Cookie

El `RolePrincipal` del objeto `IsInRole(roleName)` llamadas al método `Roles.GetRolesForUser` para obtener los roles del usuario con el fin de determinar si el usuario es miembro de *roleName*. Cuando se usa el `SqlRoleProvider`, esto da como resultado una consulta a la base de datos de almacén de rol. Al usar reglas de autorización de dirección URL basada en roles el `RolePrincipal`del `IsInRole` se llamará al método en cada solicitud a una página que está protegida por las reglas de autorización de dirección URL basada en roles. En lugar de tener que buscar la información de roles en la base de datos en cada solicitud, el marco de Roles incluye una opción para almacenar en caché los roles del usuario en una cookie.

Si el marco de trabajo de Roles está configurado para almacenar en caché los roles del usuario en una cookie, el `RoleManagerModule` crea la cookie durante la canalización de ASP.NET [ `EndRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Se usa esta cookie en solicitudes posteriores en la `PostAuthenticateRequest`, que es cuando el `RolePrincipal` se crea el objeto. Si la cookie es válida y no ha expirado, los datos de la cookie se analiza y se usa para rellenar los roles del usuario, lo que ahorra el `RolePrincipal` de tener que realizar una llamada a la `Roles` clase para determinar los roles del usuario. Figura 2 se ilustra este flujo de trabajo.


[![Información de roles del usuario se puede almacenar en una Cookie para mejorar el rendimiento](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Figura 2**: rol de información pueden almacenarse del usuario en una Cookie para mejorar el rendimiento ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image6.png))


De forma predeterminada, el mecanismo de cookies de la memoria caché de rol está deshabilitado. Se puede habilitar a través de la `<roleManager>` marcado de configuración en `Web.config`. Explicamos cómo utilizar el [ `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx) para especificar los proveedores de funciones en el <a id="_msoanchor_4"> </a> [ *creación y administración de funciones* ](creating-and-managing-roles-cs.md) tutorial, por lo que ya debería tener este elemento de la aplicación `Web.config` archivo. La configuración de cookies de la memoria caché de rol se especifica como atributos de la `<roleManager>` elemento y se resumen en la tabla 1.

> [!NOTE]
> Los valores de configuración aparece en la tabla 1 especifican las propiedades de la cookie de caché de rol resultante. Para obtener más información sobre las cookies, cómo funcionan y sus diversas propiedades, lea [este tutorial Cookies](http://www.quirksmode.org/js/cookies.html).


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Descripción</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Un valor booleano que indica si se utiliza el almacenamiento en caché de cookies. Tiene como valor predeterminado `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     El nombre de la cookie de la memoria caché de rol. El valor predeterminado es ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                La ruta de acceso para el nombre de cookie de roles. El atributo de ruta de acceso permite que un desarrollador limitar el ámbito de una cookie para una jerarquía de directorios concreta. El valor predeterminado es "/", que indica al explorador para enviar la cookie de vale de autenticación a cualquier solicitud realizada al dominio.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indica las técnicas que se usan para proteger la cookie de la memoria caché de rol. Los valores permitidos son: `All` (valor predeterminado); `Encryption`; `None`; y `Validation`. Hacer referencia al paso 3 de la <a id="_anchor_5"> </a> [ *configuración de autenticación de formularios y temas avanzados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) tutorial para obtener más información sobre estos niveles de protección.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Un valor booleano que indica si se requiere una conexión SSL para transmitir la cookie de autenticación. El valor predeterminado es `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Un valor booleano que indica que si el tiempo de espera de la cookie se restablece cada vez que el usuario visita el sitio durante una sola sesión. El valor predeterminado es `false`. Este valor solo es pertinente cuando `createPersistentCookie` está establecido en `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Especifica el tiempo, en minutos, tras el cual expira la cookie del vale de autenticación. El valor predeterminado es `30`. Este valor solo es pertinente cuando `createPersistentCookie` está establecido en `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Un valor booleano que especifica si la cookie de la memoria caché de rol es una cookie de sesión o una cookie persistente. Si `false` (valor predeterminado), se utiliza una cookie de sesión, que se elimina cuando se cierra el explorador. Si `true`, se utiliza una cookie persistente; caduca `cookieTimeout` número de minutos después de que se ha creado o después de la visita anterior, dependiendo del valor de `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Especifica el valor de dominio de la cookie. El valor predeterminado es una cadena vacía, lo que hace que el explorador usar el dominio desde el que se emitió (por ejemplo, www.yourdomain.com). En este caso, la cookie le <strong>no</strong> enviarse al realizar solicitudes a subdominios, como admin.yourdomain.com. Si desea que la cookie que se pasará a todos los subdominios necesita personalizar el `domain` atributo, si se establece en "yourdomain.com".                                                                                                                                                 |
|    `maxCachedResults`     | Especifica el número máximo de nombres de función que se almacenan en caché en la cookie. El valor predeterminado es 25. El `RoleManagerModule` no crea una cookie para que los usuarios que pertenecen a más de `maxCachedResults` roles. Por lo tanto, el `RolePrincipal` del objeto `IsInRole` método usará la `Roles` clase para determinar los roles del usuario. El motivo `maxCachedResults` existe porque muchos de los agentes de usuario no permiten que las cookies mayores que 4.096 bytes. Por lo que este extremo está pensado para reducir la probabilidad de superar esta limitación de tamaño. Si tiene nombres de función muy largos, es aconsejable considere la posibilidad de especificar un menor `maxCachedResults` valor; contrariwise, si tiene nombres de función muy corto, probablemente puede aumentar este valor. |

**Tabla 1:** las opciones de configuración de cookies de caché de rol

Vamos a configurar nuestra aplicación para usar las cookies de caché de rol no persistentes. Para lograr esto, actualice el `<roleManager>` elemento `Web.config` para incluir los siguientes atributos relacionados con la cookie:

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

He actualizado el `<roleManager>` elemento mediante la adición de tres atributos: `cacheRolesInCookie`, `createPersistentCookie`, y `cookieProtection`. Estableciendo `cacheRolesInCookie` a `true`, el `RoleManagerModule` ahora almacenará en caché automáticamente los roles del usuario en una cookie en lugar de tener que buscar información de roles del usuario en cada solicitud. Establezca explícitamente la `createPersistentCookie` y `cookieProtection` atributos a `false` y `All`, respectivamente. Técnicamente, no era necesario especificar valores para estos atributos, ya que he asignado simplemente los valores predeterminados, pero colocarlos aquí para que sea explícitamente borrar que no estoy usando las cookies persistentes y de que la cookie es cifrada y validan.

Eso es todo esto! Aquí en adelante, el marco de trabajo Roles almacenará en caché los roles de los usuarios en las cookies. Si el explorador del usuario no es compatible con las cookies, o si sus cookies se eliminan o se pierde, de algún modo, no es gran cosa: la `RolePrincipal` objeto usará simplemente los `Roles` clase en el caso de que ninguna cookie (o uno válido o ha expirado) está disponible.

> [!NOTE]
> Patrones de Microsoft &amp; grupo de prácticas recomendadas no utilizar cookies de la memoria caché de rol persistente. Puesto que la posesión de la cookie de la memoria caché de rol es suficiente para demostrar la pertenencia a roles, si un hacker alguna forma puede tener acceso a cookies de un usuario válido puede suplantar al usuario. La probabilidad de que esto ocurra aumenta si la cookie se conserva en el explorador del usuario. Para obtener más información sobre esta recomendación de seguridad, así como otros problemas de seguridad, consulte el [lista de preguntas de seguridad para ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Paso 1: Definir las reglas de autorización de dirección URL basada en roles

Como se describe en el <a id="_msoanchor_6"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-cs.md) tutorial, la autorización de URL ofrece un medio para restringir el acceso a un conjunto de páginas en un usuario por usuario o rol por rol base. Las reglas de autorización de dirección URL se establece en `Web.config` utilizando el [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) con `<allow>` y `<deny>` elementos secundarios. Además de las reglas de autorización relacionadas con el usuario que se describe en los tutoriales anteriores, cada `<allow>` y `<deny>` también puede incluir el elemento secundario:

- Un rol determinado
- Una lista delimitada por comas de roles

Por ejemplo, las reglas de autorización de URL conceden acceso a los usuarios de los roles de los administradores y los supervisores, pero denegación el acceso a todos los demás:

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

El `<allow>` elemento en el marcado anterior indica que se permiten las funciones de los administradores y los supervisores; el `<deny>` elemento indica que *todas* se deniegan a los usuarios.

Vamos a configurar nuestra aplicación para que la `ManageRoles.aspx`, `UsersAndRoles.aspx`, y `CreateUserWizardWithRoles.aspx` páginas solo son accesibles a los usuarios de la función Administrators, mientras que la `RoleBasedAuthorization.aspx` página sigue siendo accesible para todos los visitantes.

Para lograr esto, empiece agregando un `Web.config` del archivo a la `Roles` carpeta.


[![Agregue un archivo Web.config en el directorio de Roles](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Figura 3**: agregar un `Web.config` del archivo a la `Roles` directorio ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image9.png))


A continuación, agregue el siguiente marcado de configuración para `Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

El `<authorization>` elemento en el `<system.web>` sección indica que solo los usuarios del rol de los administradores pueden acceder a los recursos ASP.NET en el `Roles` directory. El `<location>` elemento define un conjunto alternativo de reglas de autorización de dirección URL para el `RoleBasedAuthorization.aspx` página, lo que permite a todos los usuarios visitar la página.

Después de guardar los cambios realizados en `Web.config`, inicie sesión como un usuario que no está en el rol de administradores y, a continuación, intentar visitar una de las páginas protegidas. El `UrlAuthorizationModule` detectará que no tiene permiso para visitar el recurso solicitado; por lo tanto, el `FormsAuthenticationModule` le redirigirá a la página de inicio de sesión. La página de inicio de sesión, a continuación, le redirigirá a la `UnauthorizedAccess.aspx` página (consulte la figura 4). Esta redirección final de la página de inicio de sesión a `UnauthorizedAccess.aspx` se produce debido a código que se ha agregado a la página de inicio de sesión en el paso 2 de la <a id="_msoanchor_7"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-cs.md) tutorial. En concreto, la página de inicio de sesión redirige automáticamente a cualquier usuario autenticado `UnauthorizedAccess.aspx` si la cadena de consulta contiene un `ReturnUrl` parámetro, como este parámetro indica que el usuario ha llegado en la página de inicio de sesión después de intentar ver una página no fue autorización para ver.


[![Solo los usuarios de la función Administrators puedan ver las páginas protegidas](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Figura 4**: solo los usuarios de la función Administrators puedan ver las páginas protegidas ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image12.png))


Cierre sesión y, a continuación, inicie sesión como un usuario que está en la función Administrators. Ahora debería poder ver las tres páginas protegidas.


[![Puede visitar Tito la UsersAndRoles.aspx página porque se encuentre en el rol de administradores](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Figura 5**: Tito puede visitar el `UsersAndRoles.aspx` página porque es de la función Administrators ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image15.png))


> [!NOTE]
> Al especificar las reglas de autorización de dirección URL: para roles o usuarios, es importante tener en cuenta que las reglas son analizado uno a uno, de arriba abajo. Tan pronto como se encuentra una coincidencia, se concede o deniega el acceso al usuario, según si se encontró la coincidencia en una `<allow>` o `<deny>` elemento. **Si se encuentra ninguna coincidencia, el usuario se concede acceso.** Por lo tanto, si desea restringir el acceso a una o varias cuentas de usuario, es esencial que utilice un `<deny>` como el último elemento en la configuración de autorización de dirección URL. **Si las reglas de autorización de dirección URL no incluyen un**`<deny>`**elemento, todos los usuarios se les concederá acceso.** Para obtener una explicación más exhaustiva sobre cómo se analizan las reglas de autorización de dirección URL, hacer referencia a la "un vistazo a cómo la `UrlAuthorizationModule` usa las reglas de autorización para conceder o denegar el acceso" sección de la <a id="_msoanchor_8"> </a> [  *Autorización basada en usuario* ](../membership/user-based-authorization-cs.md) tutorial.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Paso 2: Limitar la funcionalidad basada en Roles del usuario con sesión iniciada actualmente

Se permite facilita la autorización URL fácil especificar autorización general que ese estado de reglas de qué las identidades y cuáles se deniegan al ver una página determinada (o todas las páginas en una carpeta y sus subcarpetas). Sin embargo, en algunos casos podemos queremos permitir que todos los usuarios visitan una página, pero también limitar la funcionalidad de la página basada en roles del usuario visita. Esto puede implicar ocultando o mostrando datos basados en el rol del usuario, o que ofrecen una funcionalidad adicional a los usuarios que pertenecen a un rol determinado.

Estas reglas de autorización basada en roles de grano fino pueden implementarse de forma declarativa o mediante programación (o a través de alguna combinación de ambos). En la siguiente sección veremos cómo implementar la autorización declarativa muy específicos mediante el control LoginView. A continuación, exploraremos las técnicas de programación. Antes de que podemos adentrarnos en aplicar reglas de autorización de grano fino, sin embargo, primero es necesario crear una página cuya funcionalidad depende del rol del usuario visita.

Vamos a crear una página que enumera todas las cuentas de usuario en el sistema en un control GridView. El control GridView incluirá el nombre de usuario de cada usuario, dirección de correo electrónico, última fecha de inicio de sesión y comentarios sobre el usuario. Además de mostrar información de cada usuario, el control GridView incluirá editar y eliminar las capacidades. Se creará inicialmente esta página con la modificación y eliminar funciones disponibles para todos los usuarios. En las secciones "Utilizando el Control LoginView" y "Limitar la funcionalidad mediante programación" vemos cómo habilitar o deshabilitar estas características basadas en rol del usuario visitante.

> [!NOTE]
> La página ASP.NET que se van a compilar, usa un control GridView para mostrar las cuentas de usuario. Desde el tutorial de esta serie se centra en los roles, las cuentas de usuario, autorización y autenticación de formularios, no quiero dedicar demasiado tiempo en analizar el funcionamiento interno del control GridView. Aunque este tutorial proporciona instrucciones paso a paso específicas para la configuración de esta página, no inspeccionaremos los detalles de por qué se realizaron determinadas elecciones o lo tienen determinadas propiedades de efecto en la salida representada. Para obtener un análisis completo del control GridView, visite mi *[trabajar con datos en ASP.NET 2.0](../../data-access/index.md)* serie de tutoriales.


Comience abriendo la `RoleBasedAuthorization.aspx` página en el `Roles` carpeta. Arrastre un control GridView en la página en el diseñador y el conjunto de sus `ID` a `UserGrid`. En un momento se escribirá código que llama el `Membership.GetAllUsers` método y enlaza resultante `MembershipUserCollection` objeto en el control GridView. El `MembershipUserCollection` contiene un `MembershipUser` objeto para cada cuenta de usuario en el sistema; `MembershipUser` objetos tienen propiedades como `UserName`, `Email`, `LastLoginDate`, y así sucesivamente.

Antes de escribir el código que enlaza las cuentas de usuario a la cuadrícula, vamos a definir en primer lugar los campos de GridView. En las etiquetas inteligentes de GridView, haga clic en el vínculo "Editar columnas" para iniciar el cuadro de diálogo cuadro (consulte la figura 6). Desde aquí, desactive la casilla de verificación "generar campos automáticamente" en la esquina inferior izquierda. Puesto que deseamos que este control GridView para incluir la edición y eliminación de recursos, agregue un CommandField y establecer su `ShowEditButton` y `ShowDeleteButton` propiedades en True. A continuación, agregue cuatro campos para mostrar el `UserName`, `Email`, `LastLoginDate`, y `Comment` propiedades. Usar un BoundField para las dos propiedades de solo lectura (`UserName` y `LastLoginDate`) y TemplateFields en los dos campos modificables (`Email` y `Comment`).

Tiene la primera presentación BoundField el `UserName` propiedad; conjunto su `HeaderText` y `DataField` propiedades en "UserName". Este campo no podrán editar, así que establezca su `ReadOnly` propiedad en True. Configurar la `LastLoginDate` BoundField estableciendo su `HeaderText` para "Último inicio de sesión" y su `DataField` a "LastLoginDate". Vamos a dar formato al resultado de este BoundField para que se muestre solo la fecha (en lugar de la fecha y hora). Para ello, establezca este BoundField `HtmlEncode` propiedad en False y su `DataFormatString` propiedad en "{0:d}". Establezca también la `ReadOnly` propiedad en True.

Establecer el `HeaderText` las propiedades de los dos TemplateFields "Email" y "Comment".


[![Campos de GridView pueden configurarse mediante el cuadro de diálogo campos](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Figura 6**: campos puede ser configurado a través el cuadro de GridView de diálogo campos ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image18.png))


Ahora tenemos que definir el `ItemTemplate` y `EditItemTemplate` para el "Email" y "Comment" TemplateFields. Agregue un control Web de la etiqueta a cada uno de los `ItemTemplate` s y enlazar sus `Text` propiedades para el `Email` y `Comment` propiedades, respectivamente.

Para TemplateField "Email", agregue un cuadro de texto denominado `Email` a su `EditItemTemplate` y enlazar su `Text` propiedad a la `Email` propiedad con enlace de datos bidireccional. Agregar un RequiredFieldValidator y un control RegularExpressionValidator el `EditItemTemplate` para asegurarse de que un visitante de edición de la propiedad de correo electrónico ha escrito una dirección de correo electrónico válida. Para TemplateField "Comment", agregue un cuadro de texto de varias líneas denominado `Comment` a su `EditItemTemplate`. Establezca el cuadro de texto `Columns` y `Rows` propiedades a 40 y 4, respectivamente y, a continuación, enlazar su `Text` propiedad a la `Comment` propiedad con enlace de datos bidireccional.

Después de configurar estos TemplateFields, su marcado declarativo debe tener un aspecto similar al siguiente:

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Al editar o eliminar una cuenta de usuario, necesitamos saber a ese usuario `UserName` valor de propiedad. Establezca la GridView `DataKeyNames` propiedad en "UserName" para que esta información está disponible a través de GridView `DataKeys` colección.

Por último, agregue un control ValidationSummary a la página y establezca su `ShowMessageBox` propiedad en True y su `ShowSummary` propiedad en False. Con esta configuración, el control ValidationSummary mostrará una alerta de cliente si el usuario intenta modificar una cuenta de usuario con una dirección de correo electrónico no válida o ausente.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Ahora, hemos completado marcado declarativo de esta página. La siguiente tarea consiste en enlazar el conjunto de cuentas de usuario en el control GridView. Agregue un método denominado `BindUserGrid` a la `RoleBasedAuthorization.aspx` clase de código subyacente de la página que se enlaza el `MembershipUserCollection` devuelto por `Membership.GetAllUsers` a la `UserGrid` GridView. Llamar a este método desde el `Page_Load` controlador de eventos en la primera visita de página.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

Con este código en su lugar, visite la página a través de un explorador. Como se muestra en la figura 7, debería ver un GridView presentar información acerca de cada cuenta de usuario en el sistema.


[![UserGrid GridView muestra información acerca de cada usuario en el sistema](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Figura 7**: el `UserGrid` GridView muestra información acerca de cada usuario del sistema ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image21.png))


> [!NOTE]
> El `UserGrid` GridView enumera todos los usuarios en una interfaz no paginado. Esta interfaz cuadrícula simple no es adecuada para escenarios donde hay varias docenas o varios usuarios. Es una opción configurar el control GridView para habilitar la paginación. El `Membership.GetAllUsers` método tiene dos sobrecargas: uno que no acepta ningún parámetro de entrada y devuelve todos los usuarios y otro que acepta valores enteros para el índice de la página y el tamaño de página y devuelve solo el subconjunto especificado de los usuarios. La segunda sobrecarga puede usarse para página de forma más eficaz a través de los usuarios debido a que devuelve solo el subconjunto preciso de las cuentas de usuario en lugar de *todas* de ellos. Si tiene miles de cuentas de usuario, debe tener en cuenta una interfaz basada en filtro, que solo muestra aquellos usuarios cuyo nombre de usuario comienza con un carácter seleccionado, por ejemplo. El [ `Membership.FindUsersByName method` ](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) es ideal para crear una interfaz de usuario basada en filtros. Veremos la creación de esta interfaz en un futuro tutorial.


El control GridView ofrece integradas de edición y eliminación de soporte técnico cuando el control se enlaza a un control de origen de datos configurado correctamente, como SqlDataSource o ObjectDataSource. El `UserGrid` GridView, sin embargo, tiene sus datos enlazados mediante programación; por lo tanto, nos debemos escribir el código para llevar a cabo estas dos tareas. En concreto, necesitamos crear controladores de eventos para el control de GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, y `RowDeleting` eventos, que se desencadenan cuando un visitante de GridView Edit, Cancelar, Update, o eliminar botones.

Empiece por crear los controladores de eventos para el control de GridView `RowEditing`, `RowCancelingEdit`, y `RowUpdating` eventos y, a continuación, agregue el código siguiente:

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

El `RowEditing` y `RowCancelingEdit` controladores de eventos basta con establecer la GridView `EditIndex` propiedad y, a continuación, volver a vincular las cuentas de la lista de los usuarios a la cuadrícula. Las cosas interesantes que se produce en el `RowUpdating` controlador de eventos. Este controlador de eventos se inicia al garantizar que los datos es válidos y, a continuación, toma el `UserName` valor de la cuenta de usuario modificada desde la `DataKeys` colección. El `Email` y `Comment` cuadros de texto en 'TemplateFields dos `EditItemTemplate` s, a continuación, mediante programación hace referencia. Sus `Text` propiedades contienen la dirección de correo electrónico editado y el comentario.

Para actualizar una cuenta de usuario a través de la API de pertenencia se debe obtener la información del usuario, lo que hacemos a través de una llamada a `Membership.GetUser(userName)`. El valor devuelto `MembershipUser` del objeto `Email` y `Comment` propiedades, a continuación, se actualizan con los valores especificados en los dos cuadros de texto de la interfaz de edición. Por último, estas modificaciones se guardan con una llamada a [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). El `RowUpdating` completa de controlador de eventos al revertir el control GridView a su interfaz de edición previamente.

A continuación, cree el `RowDeleting` controlador de eventos y, a continuación, agregue el código siguiente:

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

El controlador de eventos anterior empezará por tomar el `UserName` valor desde la GridView `DataKeys` colección; esto `UserName` valor, a continuación, se pasa a la clase de pertenencia [ `DeleteUser` método](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). El `DeleteUser` método elimina la cuenta de usuario del sistema, incluidos los datos de pertenencia relacionados (por ejemplo, los roles que pertenece este usuario). Después de eliminar el usuario, la cuadrícula `EditIndex` se establece en -1 (en caso de que el usuario hizo clic en eliminar mientras otra fila estaba en modo de edición) y el `BindUserGrid` se llama al método.

> [!NOTE]
> El botón Eliminar no requiere a algún tipo de confirmación del usuario antes de eliminar la cuenta de usuario. Le sugiero que agregar algún tipo de confirmación del usuario para reducir las posibilidades de una cuenta que se eliminen accidentalmente. Una de las maneras más fáciles para confirmar una acción es a través de un cuadro de diálogo de confirmación del lado cliente. Para obtener más información sobre esta técnica, consulte [adición del lado cliente confirmación cuando se elimina](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Compruebe que esta página funciona según lo previsto. Debe ser capaz de modificar cualquier dirección de correo electrónico y el comentario, así como eliminar cualquier cuenta de usuario. Puesto que la `RoleBasedAuthorization.aspx` página sea accesible para todos los usuarios, cualquier usuario – visitantes anónimos incluso – puede visitar esta página y editar y eliminar cuentas de usuario. Vamos a actualizar esta página para que solo los usuarios de los roles de los supervisores y a los administradores pueden editar dirección de correo electrónico del usuario y el comentario, y solo los administradores pueden eliminar una cuenta de usuario.

La sección "Usar el Control LoginView" examina usando el control LoginView para mostrar instrucciones específicas para el rol del usuario. Si una persona en la función Administrators visita esta página, se mostrará las instrucciones sobre cómo editar y eliminar usuarios. Si un usuario de la función de los supervisores llega a esta página, se mostrará las instrucciones sobre cómo editar los usuarios. Y si el visitante es anónimo o no está en los supervisores o los administradores de rol, se mostrará un mensaje que explica que no se puede editar o eliminar información de la cuenta de usuario. En la sección "Limitar la funcionalidad mediante programación" se escribirá código que muestra u oculta los botones de edición y eliminación basándose en el rol del usuario de mediante programación.

### <a name="using-the-loginview-control"></a>Uso del Control LoginView

Como hemos visto en los tutoriales anteriores, el control LoginView es útil para mostrar distintas interfaces para los usuarios autenticados y anónimos, pero también se puede usar el control LoginView para mostrar un marcado diferente en función de los roles del usuario. Vamos a usar un control LoginView para mostrar instrucciones diferentes según el rol del usuario visitante.

Comience por agregar una LoginView anterior el `UserGrid` GridView. Como se ha explicado anteriormente, el control LoginView tiene dos plantillas integradas: `AnonymousTemplate` y `LoggedInTemplate`. Escriba un mensaje breve en ambas plantillas que informa al usuario que no se puede editar o eliminar cualquier información de usuario.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

Además el `AnonymousTemplate` y `LoggedInTemplate`, puede incluir el control LoginView *RoleGroups*, que son plantillas específicas de funciones. Cada RoleGroup contiene una propiedad única, `Roles`, que especifica qué roles se aplica el RoleGroup. El `Roles` propiedad puede establecerse una única función (por ejemplo, "administradores") o a una lista delimitada por comas de funciones (por ejemplo, "administradores, los supervisores").

Para administrar los grupos de funciones, haga clic en el vínculo "Editar RoleGroups" desde etiqueta del control inteligente para mostrar el Editor de colección RoleGroup. Agregue dos nuevos grupos de funciones. Establecer el RoleGroup primera `Roles` propiedad en "Administradores" y el segundo a "Supervisores".


[![Administrar plantillas de rol específicos de LoginView a través del Editor de colección RoleGroup.](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Figura 8**: administración específica de la función plantillas mediante el Editor de LoginView Kolekce RoleGroup. ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image24.png))


Haga clic en Aceptar para cerrar el Editor de colección RoleGroup; Esto actualiza el marcado declarativo de LoginView para incluir un `<RoleGroups>` sección con un `<asp:RoleGroup>` elemento secundario para cada RoleGroup definido en el Editor de la colección RoleGroup. Además, la lista desplegable "Vistas" lista de etiquetas inteligentes de LoginView - que inicialmente se incluye solamente el `AnonymousTemplate` y `LoggedInTemplate` : ahora incluye también los grupos de funciones se ha agregado.

Edite los grupos de funciones para que los usuarios del rol de los supervisores son instrucciones que aparecen en la edición de las cuentas de usuario, mientras que los usuarios del rol administradores se muestran instrucciones para modificar y eliminar. Después de realizar estos cambios, el marcado declarativo de su LoginView debe ser similar al siguiente.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Después de realizar estos cambios, guarde la página y, a continuación, se visita a través de un explorador. En primer lugar, visite la página como usuario anónimo. Debe mostrarse el mensaje, "no se registran en el sistema. Por lo tanto, no se puede modificar o eliminar cualquier información de usuario." A continuación, inicie sesión como un usuario autenticado, pero que no está ni en el rol de los supervisores ni los administradores. Ahora debería ver el mensaje, "no es un miembro de los roles de los supervisores o administradores. Por lo tanto, no se puede modificar o eliminar cualquier información de usuario."

A continuación, inicie sesión como un usuario que sea un miembro del rol de los supervisores. Esta vez, verá los supervisores específica de la función del mensaje (consulte la figura 9). Y si inicia sesión como un usuario en los administradores de rol debería ver los administradores de rol específica del mensaje (consulte la figura 10).


[![Bruce se muestra el mensaje de rol específicos de los supervisores](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Figura 9**: Bruce se muestra el mensaje de rol específicos de los supervisores ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image27.png))


[![Tito se muestra el mensaje de rol específicos de administradores](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Figura 10**: Tito se muestra el mensaje de rol específicos de los administradores ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image30.png))


Como las capturas de pantalla en las figuras 9 y 10 mostrar, el LoginView sólo representa una plantilla, incluso si se aplican varias plantillas. Bruce y Tito se registran en los usuarios, aunque el LoginView representa sólo el RoleGroup coincidente y no el `LoggedInTemplate`. Además, Tito pertenece a los roles de administradores y los supervisores, aunque el control LoginView representa la plantilla de rol específicos de administradores en lugar de los supervisores uno.

Figura 11 se ilustra el flujo de trabajo usa el control LoginView para determinar qué plantilla se debe representar. Tenga en cuenta que si hay más de uno RoleGroup especificado, la plantilla de LoginView representa el *primera* RoleGroup que coincida con. En otras palabras, si hubiéramos situamos el RoleGroup supervisores como el primer RoleGroup y los administradores como el segundo, a continuación, cuando Tito visita esta página debería ver el mensaje de supervisores.


[![Flujo de trabajo del Control LoginView para determinar qué plantilla se debe representar](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Figura 11**: flujo de trabajo del LoginView Control el para determinar qué plantilla que se representarán ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitar la funcionalidad mediante programación

Mientras el control LoginView muestra instrucciones diferentes según el rol del usuario visita la página, permanecen visibles para todos los botones Editar y Cancelar. Es necesario ocultar los botones Editar y eliminar mediante programación para los visitantes anónimos y los usuarios que se encuentran en los supervisores ni los administradores de rol. Se debe ocultar el botón Eliminar para todos los usuarios que no sea un administrador. Con el fin de realizar esta acción se va a escribir un fragmento de código que hace referencia mediante programación a la CommandField editar y eliminar LinkButtons y conjuntos de sus `Visible` propiedades a `false`, si es necesario.

Es la manera más fácil de hacer referencia mediante programación a los controles en un CommandField convertirlo a una plantilla. Para ello, haga clic en el vínculo "Editar columnas" de la etiqueta inteligente de GridView, seleccione el CommandField desde la lista de campos actuales y haga clic en el vínculo "Convert this field into a TemplateField". Esto convierte el CommandField en TemplateField con un `ItemTemplate` y `EditItemTemplate`. El `ItemTemplate` contiene la edición y eliminar LinkButtons mientras el `EditItemTemplate` aloja la actualización y cancelar LinkButtons.


[![Convertir el CommandField en TemplateField](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Figura 12**: convertir el CommandField en TemplateField ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image36.png))


Actualizar la edición y eliminar LinkButtons en el `ItemTemplate`, estableciendo su `ID` propiedades en valores de `EditButton` y `DeleteButton`, respectivamente.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Cada vez que los datos están enlazados a GridView, el control GridView enumera los registros en su `DataSource` propiedad y genera correspondiente `GridViewRow` objeto. Como cada `GridViewRow` se crea un objeto, el `RowCreated` desencadena el evento. Para ocultar los botones Editar y eliminar los usuarios no autorizados, es necesario crear un controlador de eventos para este evento y hacer referencia mediante programación la edición y eliminar LinkButtons, establecer sus `Visible` propiedades según corresponda.

Crear un controlador de eventos el `RowCreated` eventos y, a continuación, agregue el código siguiente:

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

Tenga en cuenta que el `RowCreated` evento se desencadena para *todas* de las filas de GridView, incluido el encabezado, el pie de página, la interfaz de paginación y así sucesivamente. Sólo queremos mediante programación para hacer referencia a la edición y eliminar LinkButtons si se trata de una fila de datos no está en modo de edición (ya que la fila en modo de edición tiene botones Actualizar y Cancelar, en lugar de editar y eliminar). Esta comprobación se controla mediante el `if` instrucción.

Si se trata de una fila de datos que no está en modo de edición, se hace referencia a la edición y eliminar LinkButtons y sus `Visible` propiedades se establecen basándose en los valores booleanos devueltos por la `User` del objeto `IsInRole(roleName)` método. El objeto de usuario que hace referencia a la entidad de seguridad creado por el `RoleManagerModule`; por lo tanto, el `IsInRole(roleName)` método usa la API de Roles para determinar si el visitante actual pertenece a *roleName*.

> [!NOTE]
> Podríamos haber usado la clase Roles directamente, sustituyendo la llamada a `User.IsInRole(roleName)` con una llamada a la [ `Roles.IsUserInRole(roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Decidí usar el objeto principal `IsInRole(roleName)` método en este ejemplo porque es más eficaz que usar directamente la API de Roles. Anteriormente en este tutorial, hemos configurado el Administrador de roles para almacenar en caché los roles del usuario en una cookie. Esto se almacena en caché datos de la cookie solo se utiliza cuando la entidad de seguridad `IsInRole(roleName)` se llama al método; llamadas directas a la API de Roles siempre implican un viaje a la tienda de rol. Incluso si no se almacenan las funciones en una cookie, una llamada a un objeto de entidad `IsInRole(roleName)` método normalmente es más eficaz porque cuando se llama por primera vez durante una solicitud almacena en caché los resultados. La API de Roles, por otro lado, no realiza ningún almacenamiento en caché. Dado que el `RowCreated` evento se desencadena una vez para cada fila en el control GridView, mediante `User.IsInRole(roleName)` implica un solo viaje en el almacén de roles, mientras que `Roles.IsUserInRole(roleName)` requiere *N* viajes, donde *N* es el número de cuentas de usuario que se muestran en la cuadrícula.


El botón Editar `Visible` propiedad está establecida en `true` si el usuario accedió a esta página está en el rol de administradores o los supervisores; en caso contrario se establece en `false`. El botón Eliminar `Visible` propiedad está establecida en `true` sólo si el usuario está en la función Administrators.

Pruebe esta página a través de un explorador. Si visita la página como un visitante anónimo o como un usuario que no sea un Supervisor ni un administrador, el CommandField está vacío; todavía existe, pero como una delgada Silver sin editar o eliminar botones.

> [!NOTE]
> Es posible ocultar el CommandField por completo cuando una no Supervisor y sin privilegios de administrador es visitar la página. Dejar como un ejercicio para el lector.


[![La edición y eliminar botones están ocultos para que no son supervisores y que no son administradores](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Figura 13**: botones eliminar y editar el están ocultos para no supervisores y no administradores ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image39.png))


Si visita un usuario que pertenece a la función de los supervisores (pero no al rol de administrador), ve solo el botón Editar.


[![Mientras que el botón Editar está disponible para los supervisores, se oculta el botón Eliminar](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Figura 14**: botón Editar the, mientras que está disponible para los supervisores, se oculta el botón Eliminar ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image42.png))


Y si un administrador visita, tiene acceso a los botones Editar y eliminar.


[![El editar y eliminar botones están disponibles sólo para administradores](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Figura 15**: botones eliminar y editar el están disponibles solo para los administradores ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Paso 3: Aplicar reglas de autorización basada en funciones a las clases y métodos

En el paso 2 que se ha limitado editar capacidades a los usuarios de los roles de los supervisores y a los administradores y eliminar únicamente las capacidades a los administradores. Esto se logra al ocultar los elementos de interfaz de usuario asociado para los usuarios no autorizados a través de técnicas de programación. Dichas medidas no garantiza que un usuario no autorizado no se puede realizar una acción con privilegios. Puede haber elementos de la interfaz de usuario que se agreguen posteriormente o que se haya olvidado de ocultar por los usuarios no autorizados. O bien, un hacker puede detectar alguna otra forma de obtener la página ASP.NET para ejecutar el método deseado.

Es una manera sencilla de asegurarse de que un usuario no autorizado no puede tener acceso a un elemento determinado de funcionalidad decorar esa clase o método con el [ `PrincipalPermission` atributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Cuando el tiempo de ejecución de .NET usa una clase o ejecuta uno de sus métodos, comprueba para asegurarse de que el contexto de seguridad actual tiene permiso. El `PrincipalPermission` atributo proporciona un mecanismo a través del cual podemos definir estas reglas.

Hemos examinado utilizando el `PrincipalPermission` atributo nuevo en el <a id="_msoanchor_9"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-cs.md) tutorial. En concreto, hemos visto cómo decorar la GridView `SelectedIndexChanged` y `RowDeleting` controlador de eventos para que podría solo pueden ejecutar los usuarios autenticados y Tito, respectivamente. El `PrincipalPermission` atributo funciona igual de bien con los roles.

Vamos a demostrar el uso de la `PrincipalPermission` atributo en el control de GridView `RowUpdating` y `RowDeleting` controladores de eventos para prohibir la ejecución de los usuarios no autorizados. Todo lo que necesitamos hacer es agregar el atributo apropiado de la parte superior de cada definición de función:

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

El atributo para el `RowUpdating` controlador de eventos dicta que solo los usuarios de los roles de los administradores o los supervisores pueden ejecutar el controlador de eventos, mientras que el atributo en el `RowDeleting` controlador de eventos limita la ejecución a los usuarios de los administradores rol.


> [!NOTE]
> El `PrincipalPermission` atributo se representa como una clase en el `System.Security.Permissions` espacio de nombres. No olvide agregar un `using System.Security.Permissions` instrucción en la parte superior del archivo de clase de código subyacente para importar este espacio de nombres.


Si, de algún modo, no administrador intenta ejecutar la `RowDeleting` controlador de eventos o si un intentos no Supervisor o sin privilegios de administrador para ejecutar el `RowUpdating` controlador de eventos, se producirá el runtime de .NET un `SecurityException`.


[![Si el contexto de seguridad no está autorizado para ejecutar el método, se produce una excepción SecurityException](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Figura 16**: si el contexto de seguridad no está autorizado para ejecutar el método, un `SecurityException` se produce ([haga clic aquí para ver imagen en tamaño completo](role-based-authorization-cs/_static/image48.png))


Además de las páginas ASP.NET, muchas aplicaciones también tienen una arquitectura que incluye varias capas, como lógica de negocios y los niveles de acceso a datos. Estas capas se implementan normalmente como bibliotecas de clases y ofrecen las clases y métodos para realizar la funcionalidad relacionada con datos y lógica de negocio. El `PrincipalPermission` atributo es útil para aplicar las reglas de autorización a estos niveles también.

Para obtener más información sobre el uso de la `PrincipalPermission` atributo para definir las reglas de autorización en las clases y métodos, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog [agregar reglas de autorización a Business y capas de datos con `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumen

En este tutorial hemos examinado cómo especificar generales y las reglas de autorización de grano fino en función de los roles del usuario. ASP. Característica de autorización de dirección URL de la red permite especificar qué identidades se permiten o deniegan el acceso a las páginas que un desarrollador de páginas. Como vimos en el <a id="_msoanchor_10"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-cs.md) autorización para URL tutorial, se pueden aplicar reglas según el usuario por el usuario. También se aplica según el rol por rol, como vimos en el paso 1 de este tutorial.

De forma declarativa o mediante programación, se pueden aplicar las reglas de autorización de grano fino. En el paso 2 analizamos utilizando la característica de grupos de funciones del control LoginView para representar diferentes resultados basados en roles del usuario que realiza la visita. También analizamos formas de determinar mediante programación si un usuario pertenece a un rol específico y cómo ajustar la funcionalidad de la página según sea necesario.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Agregar reglas de autorización al negocio y capas de datos mediante `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Examen de ASP.NET de 2.0 pertenencia a Roles y perfil: trabajar con Roles](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Lista de preguntas de seguridad para ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentación técnica de la `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial incluyen Suchi Banerjee y Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](assigning-roles-to-users-cs.md)
> [Siguiente](creating-and-managing-roles-vb.md)
