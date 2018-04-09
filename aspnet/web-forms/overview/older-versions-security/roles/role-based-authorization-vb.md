---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Autorización basada en roles (VB) | Documentos de Microsoft
author: rick-anderson
description: Este tutorial comienza con un vistazo a cómo el marco de trabajo de Roles asocia los roles de usuario con su contexto de seguridad. A continuación, se examina cómo aplicar la dirección URL basada en rol...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 1ca92fd194ed36f55c58666145efe445fd92823b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="role-based-authorization-vb"></a>Autorización basada en roles (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) o [descarga de PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> Este tutorial comienza con un vistazo a cómo el marco de trabajo de Roles asocia los roles de usuario con su contexto de seguridad. A continuación, se examina cómo aplicar reglas de autorización de dirección URL basada en roles. A continuación, veremos un medio declarativo y mediante programación para modificar los datos mostrados y la funcionalidad proporcionada por una página ASP.NET.


## <a name="introduction"></a>Introducción

En el <a id="_msoanchor_1"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-vb.md) tutorial hemos visto cómo utilizar la autorización de dirección URL para especificar qué usuarios pueden visitar un conjunto determinado de páginas. Con tan solo un poco de marcado en `Web.config`, se puede indicar a ASP.NET para permitir que sólo los usuarios autenticados visitar una página. Se pudo dictan que solo los usuarios Tito y Roberto se permiten, o indicar que se permiten todos los usuarios autenticados excepto Sam.

Además de autorización de URL, también analizamos técnicas declarativas y de programación para controlar los datos que se muestran y la funcionalidad proporcionada por una página en función de la visita del usuario. En concreto, se crea una página que aparece el contenido del directorio actual. Cualquier persona puede visitar esta página, pero solo los usuarios autenticados pudieron ver el contenido de los archivos y Tito sólo puede eliminar los archivos.

Aplicar reglas de autorización en forma de usuario por el usuario puede aumentar hasta alcanzar una pesadilla de contabilidad. Un enfoque más fácil de mantener consiste en utilizar la autorización basada en roles. Las buenas noticias son que las herramientas a nuestra disposición para aplicar las reglas de autorización también funcionan bien con funciones como lo hacen las cuentas de usuario. Las reglas de autorización de dirección URL pueden especificar roles en lugar de los usuarios. El control LoginView, que representa un resultado diferente para los usuarios autenticados y anónimos, puede configurarse para mostrar contenido diferente en función de los roles del usuario ha iniciado sesión. Y la API de Roles incluye métodos para determinar los roles del usuario ha iniciado sesión.

Este tutorial comienza con un vistazo a cómo el marco de trabajo de Roles asocia los roles de usuario con su contexto de seguridad. A continuación, se examina cómo aplicar reglas de autorización de dirección URL basada en roles. A continuación, veremos un medio declarativo y mediante programación para modificar los datos mostrados y la funcionalidad proporcionada por una página ASP.NET. Comencemos.

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Comprender cómo funciones están asociadas con contexto de seguridad de un usuario

Cada vez que una solicitud entra en la canalización ASP.NET está asociado a un contexto de seguridad, que incluye información que identifica el solicitante. Cuando se utiliza la autenticación de formularios, un vale de autenticación se utiliza como un token de identidad. Como se explicó en la <a id="_msoanchor_2"> </a> [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-vb.md) y <a id="_msoanchor_3"> </a> [ *formularios Configuración de autenticación y temas avanzados* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) tutoriales, la `FormsAuthenticationModule` es responsable de determinar la identidad del solicitante, lo que sucede durante la [ `AuthenticateRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Si se encuentra un vale de autenticación válido y que no hayan caducado, el `FormsAuthenticationModule` descodifica para determinar la identidad del solicitante. Crea un nuevo `GenericPrincipal` objeto y se asigna a la `HttpContext.User` objeto. El propósito de una entidad de seguridad, como `GenericPrincipal`, consiste en identificar el nombre del usuario autenticado y qué roles que pertenecen a. Este propósito es evidente por el hecho de que todos los objetos de entidad de seguridad tienen un `Identity` propiedad y un `IsInRole(roleName)` método. El `FormsAuthenticationModule`, sin embargo, no está interesado en la grabación de información de las funciones y los `GenericPrincipal` crea el objeto no especifica ningún rol.

Si está habilitado el marco de trabajo de Roles, el [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) módulo HTTP de los pasos de tras la `FormsAuthenticationModule` e identifica los roles de usuario autenticado durante el [ `PostAuthenticateRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que se activa tras la `AuthenticateRequest` eventos. Si la solicitud proviene de un usuario autenticado, el `RoleManagerModule` sobrescribe el `GenericPrincipal` objeto creado por el `FormsAuthenticationModule` y lo reemplaza con un [ `RolePrincipal` objeto](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). La `RolePrincipal` clase usa la API de Roles para determinar cuáles son las funciones que pertenece el usuario.

Figura 1 muestra el flujo de trabajo de canalización ASP.NET cuando se utiliza la autenticación de formularios y el marco de trabajo de Roles. El `FormsAuthenticationModule` se ejecuta en primer lugar, identifica al usuario a través de su vale de autenticación y crea un nuevo `GenericPrincipal` objeto. Después, el `RoleManagerModule` los pasos y sobrescribe el `GenericPrincipal` objeto con un `RolePrincipal` objeto.

Si un usuario anónimo visita el sitio, ni la `FormsAuthenticationModule` ni `RoleManagerModule` crea un objeto de entidad.


[![Los eventos de la canalización de ASP.NET para un usuario autenticado cuando se usa la autenticación de formularios y el marco de trabajo de Roles](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Figura 1**: los eventos de la canalización de ASP.NET para un autenticado usuario al utilizar autenticación de formularios y el marco de Roles ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Almacenamiento en caché información de las funciones en una Cookie

El `RolePrincipal` del objeto `IsInRole(roleName)` llamadas al método `Roles`.`GetRolesForUser` Para obtener los roles para el usuario con el fin de determinar si el usuario es miembro de *roleName*. Cuando se usa el `SqlRoleProvider`, esto da como resultado de una consulta a la base de datos de almacén de rol. Al utilizar reglas de autorización de dirección URL basada en roles el `RolePrincipal`del `IsInRole` método se llamará en cada solicitud a una página que está protegida por las reglas de autorización de dirección URL basada en roles. En lugar de buscar la información de rol en la base de datos en cada solicitud, el `Roles` framework incluye una opción para almacenar en caché los roles del usuario en una cookie.

Si el marco de trabajo de Roles está configurado para almacenar en caché los roles del usuario en una cookie, el `RoleManagerModule` la cookie se crea durante la canalización ASP.NET [ `EndRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Esta cookie se usa en las solicitudes posteriores en la `PostAuthenticateRequest`, que es cuando el `RolePrincipal` se crea el objeto. Si la cookie es válida y no ha expirado, los datos de la cookie se analiza y se usa para rellenar los roles del usuario, lo que ahorra el `RolePrincipal` de tener que realizar una llamada a la `Roles` clase para determinar los roles del usuario. Figura 2 muestra este flujo de trabajo.


[![Información de las funciones de usuario puede almacenarse en una Cookie para mejorar el rendimiento](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Figura 2**: rol información se puede almacenar el usuario en una Cookie para mejorar el rendimiento ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image6.png))


De forma predeterminada, el mecanismo de cookies de la memoria caché de rol está deshabilitado. Puede habilitarse a través de la `<roleManager>`; marcado de configuración en `Web.config`. Analizamos utilizando la [ `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx) para especificar los proveedores de roles en el <a id="_msoanchor_4"> </a> [ *crear y administrar Roles* ](creating-and-managing-roles-vb.md) tutorial, por lo que debe tener ya este elemento en la aplicación `Web.config` archivo. La configuración de cookies de la memoria caché de rol se especifica como atributos de la `<roleManager>`; elemento y se resumen en la tabla 1.

> [!NOTE]
> Los valores de configuración que se muestran en la tabla 1 especifican las propiedades de la cookie de caché de rol resultante. Para obtener más información sobre las cookies, cómo funcionan y sus diversas propiedades, lea [este tutorial Cookies](http://www.quirksmode.org/js/cookies.html).


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Descripción</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Un valor booleano que indica si se usa el almacenamiento en caché de cookies. Tiene como valor predeterminado `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     El nombre de la cookie de la memoria caché de rol. El valor predeterminado es ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                La ruta de acceso para el nombre de cookie de roles. El atributo de ruta de acceso permite que un programador limitar el ámbito de una cookie a una jerarquía de directorio concreto. El valor predeterminado es "/", que informa el explorador para que envíe la cookie de vale de autenticación a cualquier solicitud que se producen en el dominio.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indica qué técnicas se utilizan para proteger la cookie de la memoria caché de rol. Los valores permitidos son: `All` (valor predeterminado); `Encryption`; `None`; y `Validation`. Hacen referencia al paso 3 de la <a id="_anchor_5"> </a> [ *configuración de autenticación de formularios y temas avanzados* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) tutorial para obtener más información sobre los niveles de protección.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Un valor booleano que indica si se requiere una conexión SSL para transmitir la cookie de autenticación. El valor predeterminado es `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Un valor booleano que indica que si el tiempo de espera de la cookie se restablece cada vez que el usuario visita el sitio durante una sola sesión. El valor predeterminado es `false`. Este valor solo es pertinente cuando `createPersistentCookie` está establecido en `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Especifica el tiempo, en minutos, tras el cual expira la cookie de vale de autenticación. El valor predeterminado es `30`. Este valor solo es pertinente cuando `createPersistentCookie` está establecido en `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Un valor booleano que especifica si la cookie de la memoria caché de rol es una cookie de sesión o una cookie persistente. Si `false` (valor predeterminado), se utiliza una cookie de sesión, que se elimina cuando se cierra el explorador. Si `true`, se utiliza una cookie persistente; que expire `cookieTimeout` número de minutos después de que se ha creado o después de la visita anterior, dependiendo del valor de `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Especifica el valor de dominio de la cookie. El valor predeterminado es una cadena vacía, que hace que el explorador que utilice el dominio desde el que se emitió (por ejemplo, www.yourdomain.com). En este caso, la cookie le <strong>no</strong> enviará al realizar solicitudes a subdominios, como admin.yourdomain.com. Si desea que la cookie que se pasan a todos los subdominios necesite personalizar la `domain` atributo, si se establece en "dominio.com".                                                                                                                                                 |
|    `maxCachedResults`     | Especifica el número máximo de nombres de función que se almacenan en caché en la cookie. El valor predeterminado es 25. El `RoleManagerModule` no crea una cookie para que los usuarios que pertenecen a más de `maxCachedResults` roles. Por lo tanto, la `RolePrincipal` del objeto `IsInRole` método usará la `Roles` clase para determinar los roles del usuario. El motivo `maxCachedResults` existe porque muchos agentes de usuario no permiten cookies superiores a 4.096 bytes. Por lo que este extremo está pensado para reducir la probabilidad de superar esta limitación de tamaño. Si tiene nombres de rol muy largos, puede que desee que considere la posibilidad de especificar un menor `maxCachedResults` valor; contrariwise, si tiene nombres de rol muy cortos, probablemente puede aumentar este valor. |

**Tabla 1**: las opciones de configuración de cookies de caché de rol

Vamos a configurar nuestra aplicación use cookies de caché de rol no persistentes. Para ello, actualice el `<roleManager>` elemento `Web.config` para incluir los siguientes atributos relacionados con la cookie:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

Actualizó la `<roleManager>`; elemento mediante la adición de tres atributos: `cacheRolesInCookie`, `createPersistentCookie`, y `cookieProtection`. Estableciendo `cacheRolesInCookie` a `true`, el `RoleManagerModule` almacenará ahora automáticamente en memoria caché los roles del usuario en una cookie en lugar de tener que buscar información de rol del usuario en cada solicitud. Establecer explícitamente el `createPersistentCookie` y `cookieProtection` atributos `false` y `All`, respectivamente. Técnicamente, no necesita especificar valores para estos atributos, ya que asigna sólo con sus valores predeterminados, pero colocar aquí para que sea explícitamente desactive que no estoy usando las cookies persistentes y que la cookie es cifran y validaron.

Eso es todo lo! De ahora en adelante, el marco de trabajo de Roles almacenará en memoria caché los roles de los usuarios en las cookies. Si el explorador del usuario no admite cookies o si sus cookies se eliminan o se pierde, de algún modo, no es merece la pena: el `RolePrincipal` objeto usaremos el término el `Roles` clase en el caso de que ninguna cookie (o uno no válido o caducado) está disponible.

> [!NOTE]
> Patrones de Microsoft &amp; grupo de procedimientos no utilizar cookies de la memoria caché de rol persistente. Puesto que la posesión de la cookie de la memoria caché de rol es suficiente para demostrar la pertenencia a roles, si un pirata informático algún modo puede obtener acceso a las cookies de un usuario válido puede suplantar al usuario. La probabilidad de que esto ocurra aumenta si la cookie se conserva en el explorador del usuario. Para obtener más información sobre esta recomendación de seguridad, así como otras cuestiones de seguridad, consulte el [lista de preguntas de seguridad para ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Paso 1: Definir reglas de autorización de dirección URL basada en roles

Como se describe en el <a id="_msoanchor_6"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-vb.md) tutorial, autorización URL ofrece un medio para restringir el acceso a un conjunto de páginas de un usuario por usuario o rol por rol base. Las reglas de autorización de dirección URL se establece en `Web.config` mediante la [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) con `<allow>` y `<deny>` los elementos secundarios. Además de las reglas de autorización relacionadas con el usuario que se describe en los tutoriales anteriores, cada `<allow>` y `<deny>` también puede incluir el elemento secundario:

- Un rol determinado
- Una lista delimitada por comas de roles

Por ejemplo, las reglas de autorización de dirección URL conceden acceso a los usuarios de las funciones de los administradores y los supervisores, pero denegación el acceso a todos los demás:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

El `<allow>` elemento en el formato anterior indica que se permiten las funciones de los administradores y los supervisores; la `<deny>`; elemento indica que *todos los* se deniegan a los usuarios.

Vamos a configurar la aplicación para que la `ManageRoles.aspx`, `UsersAndRoles.aspx`, y `CreateUserWizardWithRoles.aspx` páginas solo están accesibles para los usuarios de la función Administradores, mientras que la `RoleBasedAuthorization.aspx` página siguen siendo accesible a todos los visitantes.

Para lograr esto, empiece por agregar una `Web.config` del archivo a la `Roles` carpeta.


[![Agregue un archivo Web.config en el directorio de Roles](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Figura 3**: agregar un `Web.config` del archivo a la `Roles` directorio ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image9.png))


A continuación, agregue el siguiente marcado de configuración para `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

El `<authorization>` elemento en el `<system.web>` sección indica que solo los usuarios de la función administradores pueden tener acceso a los recursos ASP.NET en el `Roles` directory. El `<location>` elemento define un conjunto alternativo de reglas de autorización de dirección URL para el `RoleBasedAuthorization.aspx` página, lo que permite a todos los usuarios visitar la página.

Después de guardar los cambios realizados en `Web.config`, inicie sesión como un usuario que no está en la función Administradores y, a continuación, intenta visitar una de las páginas protegidas. El `UrlAuthorizationModule` detectará que no tiene permiso para visitar el recurso solicitado; por lo tanto, la `FormsAuthenticationModule` le redirigirá a la página de inicio de sesión. La página de inicio de sesión, a continuación, le redirigirá a la `UnauthorizedAccess.aspx` página (consulte la figura 4). Esta redirección final de la página de inicio de sesión a `UnauthorizedAccess.aspx` se produce debido a código que se agrega a la página de inicio de sesión en el paso 2 de la <a id="_msoanchor_7"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-vb.md) tutorial. En concreto, la página de inicio de sesión redirige automáticamente a cualquier usuario autenticado `UnauthorizedAccess.aspx` si la cadena de consulta contiene un `ReturnUrl` parámetro, como este parámetro indica que el usuario ha llegado en la página de inicio de sesión después de intentar ver una página que no era autorizado a ver.


[![Solo los usuarios de la función administradores puedan ver las páginas protegidas](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Figura 4**: solo los usuarios de la función administradores puedan ver las páginas protegidas ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image12.png))


Cierre la sesión y, a continuación, inicie sesión como un usuario que se encuentra en la función Administradores. Ahora debería poder ver las tres páginas protegidas.


[![Puede visitar Tito de que la UsersAndRoles.aspx página porque se encuentra en la función administradores](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Figura 5**: Tito puede visitar el `UsersAndRoles.aspx` página porque se encuentra en la función Administradores ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> Al especificar las reglas de autorización de dirección URL: para roles o usuarios: es importante tener en cuenta que las reglas son analizado uno a uno, de arriba abajo. En cuanto se encuentra una coincidencia, se concede o deniega el acceso al usuario, dependiendo de if se encontró la coincidencia en un `<allow>` o `<deny>` elemento. **Si se encuentra ninguna coincidencia, se concede al usuario acceso.** Por consiguiente, si desea restringir el acceso a una o varias cuentas de usuario, es esencial que utilice un `<deny>` elemento como el último elemento de la configuración de autorización de dirección URL. **Si las reglas de autorización de dirección URL no incluyen un**`<deny>`**elemento, todos los usuarios se le concederá acceso.** Para obtener una explicación más exhaustiva de cómo se analizan las reglas de autorización de dirección URL, hacen referencia a la "un vistazo a cómo el `UrlAuthorizationModule` usa las reglas de autorización para conceder o denegar acceso" sección de la <a id="_msoanchor_8"> </a> [  *Autorización basada en usuario* ](../membership/user-based-authorization-vb.md) tutorial.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Paso 2: Limitar la funcionalidad basada en Roles del usuario actualmente ha iniciado sesión

Se permite hace de autorización de dirección URL más sencilla especificar la autorización más reglas a ese estado qué identidades y cuáles se deniegan de ver una página determinada (o todas las páginas en una carpeta y sus subcarpetas). Sin embargo, en ciertos casos, podemos queremos permitir a los usuarios visitar una página, pero limitar la funcionalidad de la página basada en roles del usuario visitando. Esto puede implicar visible u ocultando datos según el rol del usuario, o que ofrecen una funcionalidad adicional a los usuarios que pertenecen a un rol determinado.

Estas reglas de autorización basada en roles concretos pueden implementarse mediante declaración o mediante programación (o a través de una combinación de ambos). En la siguiente sección veremos cómo implementar la autorización declarativa concretos mediante el control LoginView. A continuación, exploraremos técnicas de programación. Antes de que podemos observar aplicar reglas de autorización concretos, sin embargo, primero necesitamos crear una página cuya funcionalidad depende del rol del usuario visita.

Vamos a crear una página que enumera todas las cuentas de usuario en el sistema en un control GridView. GridView incluirá el nombre de usuario, dirección de correo electrónico, última fecha de inicio de sesión y comentarios sobre el usuario de cada usuario. Además de mostrar la información de cada usuario, GridView incluirá editar y eliminar las capacidades. Se creará inicialmente esta página con la modificación y eliminar funciones disponibles para todos los usuarios. En las secciones "Usando el Control LoginView" y "Limitar la funcionalidad mediante programación" veremos cómo habilitar o deshabilitar estas características basadas en rol del usuario visitando.

> [!NOTE]
> La página ASP.NET que se van a compilar, usa un control GridView para mostrar las cuentas de usuario. Desde este tutorial serie se centra en la autenticación de formularios, autorización, cuentas de usuario y roles, no deseo invierta demasiado tiempo hablar sobre el funcionamiento interno del control GridView. Aunque este tutorial proporciona instrucciones paso a paso específicas para la configuración de esta página, no profundizar en los detalles de por qué se realizaron determinadas opciones o lo tienen determinadas propiedades de efecto en la salida representada. Para obtener un examen exhaustivo del control GridView, visite mi *[trabajar con datos en ASP.NET 2.0](../../data-access/index.md)* serie de tutoriales.


Comience abriendo la `RoleBasedAuthorization.aspx` página en el `Roles` carpeta. Arrastre un control GridView en la página en el diseñador y el conjunto de sus `ID` a `UserGrid`. En un momento en que se escribirá el código que llama a la `Membership`.`GetAllUsers` método y enlaza resultante `MembershipUserCollection` objeto a la GridView. El `MembershipUserCollection` contiene un `MembershipUser` objeto para cada cuenta de usuario en el sistema; `MembershipUser` objetos tienen propiedades como `UserName`,`Email`,`LastLoginDate` y así sucesivamente.

Antes de escribir el código que enlaza las cuentas de usuario a la cuadrícula, vamos a definir campos de GridView. En las etiquetas inteligentes del control GridView, haga clic en el vínculo "Editar columnas" para iniciar el cuadro de diálogo campos cuadro (consulte la figura 6). Desde aquí, desactive la casilla de verificación "generar campos automáticamente" en la esquina inferior izquierda. Puesto que deseamos que este GridView para incluir la edición y eliminación de recursos, agregue un CommandField y establecer su `ShowEditButton` y `ShowDeleteButton` propiedades en True. A continuación, agregue cuatro campos para mostrar la `UserName`, `Email`, `LastLoginDate`, y `Comment` propiedades. Usar un BoundField para las dos propiedades de sólo lectura (`UserName` y `LastLoginDate`) y TemplateFields para los dos campos modificables (`Email` y `Comment`).

Tiene la primera presentación BoundField el `UserName` propiedad; se establece su `HeaderText` y `DataField` propiedades para "UserName". Este campo no se puede editar, por tanto, establecer su `ReadOnly` propiedad en True. Configurar la `LastLoginDate` BoundField estableciendo su `HeaderText` para "Último inicio de sesión" y su `DataField` a "LastLoginDate". Vamos a dar formato al resultado de este BoundField para que se muestre solo la fecha (en lugar de la fecha y hora). Para ello, establezca este BoundField `HtmlEncode` propiedad en False y su `DataFormatString` propiedad a "{0: d}". Establecer el `ReadOnly` propiedad en True.

Establecer el `HeaderText` propiedades de la dos TemplateFields "Email" y "Comentario".


[![Campos de GridView pueden configurarse mediante el cuadro de diálogo campos](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Figura 6**: campos puede estar configurado a través del cuadro del The GridView de diálogo campos ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image18.png))


Ahora necesitamos definir la `ItemTemplate` y `EditItemTemplate` para el "Email" y "Comment" TemplateFields. Agregar un control Web Label a cada uno de los `ItemTemplates` y enlazar sus `Text` propiedades para la `Email` y `Comment` propiedades, respectivamente.

Para el TemplateField "Email", agregue un cuadro de texto denominado `Email` a su `EditItemTemplate` y enlazar sus `Text` propiedad a la `Email` propiedad con enlace de datos bidireccional. Agregar un RequiredFieldValidator y RegularExpressionValidator a la `EditItemTemplate` para asegurarse de que un visitante modificando la propiedad de correo electrónico ha escrito una dirección de correo electrónico válida. Para el TemplateField "Comment", agregue un cuadro de texto de varias líneas denominado `Comment` a su `EditItemTemplate`. Establezca el cuadro de texto `Columns` y `Rows` propiedades 40 y 4, respectivamente y, a continuación, enlazar su `Text` propiedad a la `Comment` propiedad con enlace de datos bidireccional.

Después de configurar estos TemplateFields, su marcado declarativo debe ser similar al siguiente:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Al editar o eliminar una cuenta de usuario tendrá que conocer ese usuario `UserName` valor de propiedad. Establecer la GridView `DataKeyNames` propiedad en "UserName" para que esta información está disponible a través de la GridView `DataKeys` colección.

Por último, agregue un control a la página y establezca su `ShowMessageBox` propiedad en True y su `ShowSummary` propiedad en False. Con esta configuración, el control ValidationSummary mostrará una alerta de cliente si el usuario intenta modificar una cuenta de usuario con una dirección de correo electrónico no válida o ausente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Se ha finalizado el marcado declarativo de la página. La siguiente tarea consiste en enlazar el conjunto de cuentas de usuario en GridView. Agregue un método denominado `BindUserGrid` a la `RoleBasedAuthorization.aspx` clase de código subyacente de la página que se enlaza el `MembershipUserCollection` devuelto por `Membership.GetAllUsers` a la `UserGrid` GridView. Llamar a este método desde el `Page_Load` controlador de eventos en la primera página, visite.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Con este código en su lugar, visite la página a través de un explorador. Como se muestra en la figura 7, debería ver un GridView presentar información acerca de cada cuenta de usuario en el sistema.


[![UserGrid GridView muestra información acerca de cada usuario en el sistema](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Figura 7**: el `UserGrid` GridView muestra información acerca de cada usuario en el sistema ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> El `UserGrid` GridView muestra todos los usuarios en una interfaz no paginado. Esta interfaz cuadrícula simple no es adecuada para escenarios donde hay varias docenas o varios usuarios. Una opción consiste en configurar el control GridView para habilitar la paginación. El `Membership.GetAllUsers` método tiene dos sobrecargas: una que no acepta ningún parámetro de entrada y devuelve todos los usuarios y otro que acepta valores enteros para el índice de la página y el tamaño de página y devuelve solo el subconjunto especificado de los usuarios. La segunda sobrecarga puede usarse para la página de forma más eficaz a través de los usuarios debido a que devuelve sólo el subconjunto preciso de las cuentas de usuario en lugar de *todos los* de ellos. Si tiene varios miles de cuentas de usuario, debe tener en cuenta una interfaz basada en filtros, que solo muestra los usuarios cuyo nombre de usuario comienza con un carácter seleccionado, por ejemplo. El [ `Membership.FindUsersByName` método](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) es ideal para la creación de una interfaz de usuario basada en filtros. Veremos la creación de dicha interfaz en un tutorial posterior.


El control GridView ofrece integrados de edición y eliminación de soporte técnico cuando el control se enlaza a un control de origen de datos correctamente configurados, como el SqlDataSource o ObjectDataSource. El `UserGrid` GridView, sin embargo, tiene sus datos enlazados mediante programación; por lo tanto, se debe escribir código para llevar a cabo estas dos tareas. En concreto, necesitamos crear controladores de eventos del control de GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, y `RowDeleting` eventos que se desencadenan cuando un visitante hace clic en el control de GridView Edit, Cancelar, Update, o eliminar botones.

Empiece por crear los controladores de eventos para el GridView `RowEditing`, `RowCancelingEdit`, y `RowUpdating` eventos y, a continuación, agregue el código siguiente:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

El `RowEditing` y `RowCancelingEdit` controladores de eventos basta con establecer la GridView `EditIndex` propiedad y, a continuación, vuelva a enlazar la lista de los usuarios de cuentas a la cuadrícula. Las cosas interesantes que se produce en el `RowUpdating` controlador de eventos. Este controlador de eventos inicia asegurándose de que los datos es válidos y, a continuación, toma el `UserName` valor de la cuenta de usuario editado desde el `DataKeys` colección. El `Email` y `Comment` cuadros de texto en 'TemplateFields dos `EditItemTemplate` s, a continuación, hacen referencia mediante programación. Sus `Text` propiedades contienen la dirección de correo electrónico editado y el comentario.

Para actualizar una cuenta de usuario a través de la API de pertenencia que necesitamos primero hay que obtener la información del usuario, que se realizan a través de una llamada a `Membership.GetUser(userName)`. El valor devuelto `MembershipUser` del objeto `Email` y `Comment` propiedades, a continuación, se actualizan con los valores especificados en los dos cuadros de texto de la interfaz de edición. Por último, estas modificaciones se guardan con una llamada a [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). El `RowUpdating` controlador de eventos completa revirtiendo GridView a su interfaz de edición previamente.

A continuación, cree el `RowDeleting` RowDeleting controlador de eventos y, a continuación, agregue el código siguiente:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

El controlador de eventos anterior empezará por tomar el `UserName` valor desde el GridView `DataKeys` colección; esto `UserName` , a continuación, se pasa el valor en la clase de pertenencia [ `DeleteUser` método](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). El `DeleteUser` método elimina la cuenta de usuario del sistema, incluidos los datos de pertenencia relacionados (por ejemplo, qué funciones pertenece este usuario). Después de eliminar el usuario, la cuadrícula `EditIndex` se establece en -1 (en caso de que el usuario hizo clic en eliminar mientras otra fila estaba en modo de edición) y el `BindUserGrid` se llama al método.

> [!NOTE]
> El botón Eliminar no requiere a ningún tipo de confirmación del usuario antes de eliminar la cuenta de usuario. Le recomienda que agregue alguna forma de confirmación del usuario para reducir la posibilidad de una cuenta que se eliminen accidentalmente. Una de las formas más sencillas para confirmar una acción es a través de un cuadro de diálogo de confirmación del lado cliente. Para obtener más información sobre esta técnica, consulte [Agregar cliente confirmación cuando se elimina](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Compruebe que esta página funciona según lo esperado. Podrá editar la dirección de correo electrónico de cualquier usuario y un comentario, así como eliminar cualquier cuenta de usuario. Puesto que la `RoleBasedAuthorization.aspx` página sea accesible para todos los usuarios, cualquier usuario – incluso anónimos visitantes: visite esta página y modificar y eliminar cuentas de usuario. Vamos a actualizar esta página para que solo los usuarios en los roles de los supervisores y los administradores pueden editar dirección de correo electrónico de un usuario y marque como comentario y sólo los administradores pueden eliminar una cuenta de usuario.

La sección "Usar el Control LoginView" examina usando el control LoginView para mostrar las instrucciones específicas para el rol del usuario. Si una persona con la función Administradores de visita esta página, le mostraremos instrucciones sobre cómo editar y eliminar usuarios. Si un usuario en el rol de los supervisores llega a esta página, le mostraremos instrucciones sobre cómo editar los usuarios. Y si el visitante es anónimo o no está en función de los supervisores o los administradores, se mostrará un mensaje que explica que no se puede editar o eliminar información de la cuenta de usuario. En la sección "Limitar la funcionalidad mediante programación" se escribirá el código que muestra u oculta los botones Editar y eliminar según el rol del usuario de mediante programación.

### <a name="using-the-loginview-control"></a>Uso del Control LoginView

Como hemos visto en tutoriales anteriores, el control LoginView es útil para mostrar de distintas interfaces para los usuarios autenticados y anónimos, pero el control LoginView también puede utilizarse para mostrar un marcado diferente en función de los roles del usuario. Vamos a usar un control LoginView para mostrar instrucciones diferentes según el visitando rol del usuario.

Comience por agregar una LoginView anterior la `UserGrid` GridView. Como mencionamos anteriormente, el control LoginView tiene dos plantillas integradas: `AnonymousTemplate` y `LoggedInTemplate`. Escriba un mensaje breve en ambas plantillas que informa al usuario que no se puede editar o eliminar cualquier información de usuario.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Además el `AnonymousTemplate` y `LoggedInTemplate`, puede incluir el control LoginView *RoleGroups*, que son plantillas específicas de las funciones. Cada RoleGroup contiene una propiedad única, `Roles`, que especifica qué roles se aplica el RoleGroup. El `Roles` propiedad puede establecerse en un solo rol (por ejemplo, "administradores") o a una lista delimitada por comas de funciones (por ejemplo, "administradores, los supervisores").

Para administrar la RoleGroups, haga clic en el vínculo "Editar RoleGroups" de etiqueta del control inteligente para mostrar el Editor de colección RoleGroup. Agregue dos RoleGroups nueva. Establecer el RoleGroup primera `Roles` propiedad en "Administradores" y la segunda para "Supervisores".


[![Administrar plantillas de específicas de las funciones de LoginView a través del Editor de colección RoleGroup](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Figura 8**: administrar específicas de las funciones plantillas a través de la RoleGroup Editor del LoginView de la colección ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image24.png))


Haga clic en Aceptar para cerrar el Editor de la colección RoleGroup; Esto actualiza marcado declarativo de LoginView para incluir un `<RoleGroups>` sección con un `<asp:RoleGroup>` elemento secundario para cada RoleGroup definido en el Editor de la colección RoleGroup. Además, la lista desplegable "Vistas" lista de etiquetas inteligentes de LoginView - que inicialmente se enumeran aparte de las `AnonymousTemplate` y `LoggedInTemplate` : ahora incluye también el RoleGroups agregada.

Editar el RoleGroups para que los usuarios del rol de los supervisores son instrucciones que aparecen en la edición de cuentas de usuario, mientras que los usuarios de la función administradores se muestran instrucciones para modificar y eliminar. Después de realizar estos cambios, el marcado declarativo del su LoginView debe ser similar al siguiente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Después de realizar estos cambios, guarde la página y, a continuación, se visita a través de un explorador. Visite la página por primera vez como un usuario anónimo. Debe mostrarse el mensaje, "no se registran en el sistema. Por lo tanto, no puede editar o eliminar cualquier información de usuario." A continuación, inicie sesión como un usuario autenticado, pero que no está ni en el rol de los supervisores ni los administradores. Ahora debería ver el mensaje, "no es un miembro de los roles de los supervisores o los administradores. Por lo tanto, no puede editar o eliminar cualquier información de usuario."

A continuación, inicie sesión como un usuario que sea un miembro de la función de los supervisores. Ahora debería ver los supervisores específicas de las funciones de mensaje (consulte la figura 9). Y si ha iniciado sesión como un usuario de los administradores de rol debería ver los administradores específicas de las funciones de mensaje (consulte la figura 10).


[![Bruce se muestra el mensaje de específicas de las funciones de los supervisores](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Figura 9**: Bruce se muestra el mensaje de específicas de las funciones de los supervisores ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image27.png))


[![Tito se muestra el mensaje de específicas de las funciones de los administradores](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Figura 10**: Tito se muestra el mensaje de específicas de las funciones de los administradores ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image30.png))


Como las capturas de pantalla en las figuras 9 y 10 mostrar, la LoginView representa sólo una plantilla, incluso si se aplican varias plantillas. Bruce y Tito se registran en los usuarios, aunque el LoginView representa sólo el RoleGroup coincidente y no el `LoggedInTemplate`. Además, Tito pertenece a los roles de los administradores y los supervisores, aunque el control LoginView representa la plantilla de rol específicos de administradores en lugar de los supervisores uno.

Figura 11 muestra el flujo de trabajo utilizado por el control LoginView para determinar qué plantilla se debe representar. Tenga en cuenta que si hay más de un RoleGroup especificado, se representa la plantilla de LoginView el *primer* RoleGroup que coincida con. En otras palabras, si hubiéramos colocamos el RoleGroup los supervisores como el primer RoleGroup y los administradores como el segundo, a continuación, cuando Tito visita esta página vería el mensaje de los supervisores.


[![Flujo de trabajo del Control LoginView para determinar qué plantilla se debe representar](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Figura 11**: flujo de trabajo del LoginView Control el para determinar qué plantilla de representación ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitar la funcionalidad mediante programación

Mientras que el control LoginView muestra instrucciones diferentes según el rol del usuario visita la página, permanecen visibles para todos los botones Editar y Cancelar. Es necesario ocultar los botones Editar y eliminar mediante programación para los visitantes anónimos y a los usuarios del rol de los supervisores ni los administradores. Es necesario ocultar el botón Eliminar para todos los usuarios que no sea un administrador. Para conseguirlo se escribirá un poco de código que hace referencia mediante programación a la CommandField editar y eliminar LinkButton y establece su `Visible` propiedades para `False`, si es necesario.

La manera más fácil de hacer referencia a controles en un CommandField mediante programación es conviértalo primero en una plantilla. Para ello, haga clic en el vínculo "Editar columnas" de la etiqueta inteligente de GridView, seleccione el CommandField desde la lista de campos actuales y haga clic en el vínculo "Convertir este campo en TemplateField". Esto convierte el CommandField en TemplateField con un `ItemTemplate` y `EditItemTemplate`. El `ItemTemplate` contiene la edición y eliminar LinkButton mientras el `EditItemTemplate` aloja la actualización y cancelar LinkButton.


[![Convertir el CommandField en TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Figura 12**: convertir la CommandField en TemplateField ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image36.png))


Actualizar la edición y eliminar LinkButton en el `ItemTemplate`, y establece su `ID` propiedades con valores de `EditButton` y `DeleteButton`, respectivamente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Cada vez que están enlazado a datos en GridView, GridView enumera los registros en su `DataSource` propiedad y genera su correspondiente `GridViewRow` objeto. Cada `GridViewRow` se crea el objeto, el `RowCreated` desencadena el evento. Para ocultar los botones Editar y eliminar los usuarios no autorizados, debe crear un controlador de eventos para este evento y hacer referencia mediante programación la editar y eliminar LinkButton, establecer sus `Visible` propiedades según corresponda.

Crear un controlador de eventos el `RowCreated` eventos y, a continuación, agregue el código siguiente:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Tenga en cuenta que la `RowCreated` evento se desencadena para *todos los* de las filas de GridView, incluidos el encabezado, el pie de página, la interfaz de localizador y así sucesivamente. Sólo queremos mediante programación para hacer referencia a la edición y eliminar LinkButton si estamos trabajando con una fila de datos no está en modo de edición (puesto que la fila en modo de edición tiene botones Actualizar y Cancelar en lugar de editar y eliminar). Esta comprobación se controla mediante el `If` instrucción.

Si estamos trabajando con una fila de datos que no está en modo de edición, se hace referencia a la edición y eliminar LinkButton y sus `Visible` propiedades se establecen basándose en los valores booleanos devueltos por la `User` del objeto `IsInRole(roleName)` método. El `User` objeto hace referencia a la entidad de seguridad creado por la `RoleManagerModule`; por lo tanto, la `IsInRole(roleName)` método usa la API de Roles para determinar si el visitante actual pertenece a *roleName*.

> [!NOTE]
> Podríamos haber utilizado la clase Roles directamente, reemplace la llamada a `User.IsInRole(roleName)` con una llamada a la [ `Roles.IsUserInRole(roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Decidí usar el objeto principal `IsInRole(roleName)` método en este ejemplo porque es más eficaz que utilizar la API de Roles directamente. Anteriormente en este tutorial, hemos configurado el Administrador de roles para almacenar en caché los roles del usuario en una cookie. Esto se almacena en caché datos de la cookie solo se utiliza cuando la entidad de seguridad `IsInRole(roleName)` se llama al método; llamadas directas a la API de Roles siempre implican un viaje a la tienda de rol. Incluso si los roles no se almacenan provisionalmente en una cookie, el objeto principal de llamada `IsInRole(roleName)` método normalmente es más eficaz porque cuando se llama por primera vez durante una solicitud almacena en caché los resultados. La API de Roles, por otro lado, no realiza ningún almacenamiento en caché. Dado que la `RowCreated` evento se desencadena una vez para cada fila en el control GridView, con `User.IsInRole(roleName)` implica un solo recorrido en el almacén de roles, mientras que `Roles.IsUserInRole(roleName)` requiere *N* vueltas, donde *N* es el número de cuentas de usuario que se muestran en la cuadrícula.


El botón Editar `Visible` propiedad está establecida en `True` si el usuario accedió a esta página está en la función de los administradores o los supervisores; en caso contrario se establece en `False`. El botón Eliminar `Visible` propiedad está establecida en `True` sólo si el usuario está en la función Administradores.

Pruebe esta página a través de un explorador. Si visita la página como un visitante anónimo o como un usuario que no es un Supervisor ni un administrador, el CommandField está vacía. todavía existe, pero como un fino Silver sin el modificar o eliminar botones.

> [!NOTE]
> Es posible ocultar el CommandField por completo cuando una no Supervisor y sin privilegios de administrador está visitando la página. Dejar este como un ejercicio para el lector.


[![La edición y eliminar botones están ocultos para no supervisores y no administradores](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Figura 13**: botones eliminar y editar el están ocultos para no supervisores y no administradores ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image39.png))


Si un usuario que pertenece a la función de los supervisores (pero no a la función administradores) visita, ve el botón de edición.


[![Mientras que el botón Editar no está disponible para los supervisores, se oculta el botón Eliminar](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Figura 14**: botón Editar el, mientras está disponible para los supervisores, se oculta el botón Eliminar ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image42.png))


Y si visita un administrador, tiene acceso a los botones Editar y eliminar.


[![La edición y eliminar botones están disponibles sólo para administradores](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Figura 15**: botones eliminar y editar el están disponibles solo para los administradores ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Paso 3: Aplicar las reglas de autorización basada en roles a las clases y métodos

En el paso 2 que se ha limitado Editar funciones a los usuarios en los roles de los supervisores y los administradores y eliminar capacidades sólo a los administradores. Esto se logra al ocultar los elementos de interfaz de usuario asociado para los usuarios no autorizados a través de técnicas de programación. Dichas medidas no garantizan que un usuario no autorizado podrá realizar una acción con privilegios. Puede haber elementos de la interfaz de usuario que se agreguen más tarde o que se haya olvidado de ocultar para los usuarios no autorizados. O bien, un pirata informático puede detectar alguna otra forma de obtener la página ASP.NET para ejecutar el método deseado.

Es una manera sencilla de asegurarse de que un usuario no autorizado no puede tener acceso a un elemento determinado de la funcionalidad decorar esa clase o método con el [ `PrincipalPermission` atributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Cuando el tiempo de ejecución de .NET utiliza una clase o ejecuta uno de sus métodos, comprueba para asegurarse de que el contexto de seguridad actual tiene permiso. El `PrincipalPermission` atributo proporciona un mecanismo a través del cual podemos definir estas reglas.

Analizamos utilizando la `PrincipalPermission` atributo nuevo en el <a id="_msoanchor_9"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-vb.md) tutorial. Específicamente, hemos visto cómo decorar la GridView `SelectedIndexChanged` y `RowDeleting` controlador de eventos para que podría solo pueden ejecutar los usuarios autenticados y Tito, respectivamente. El `PrincipalPermission` atributo funciona igual que con los roles.

Vamos a muestran cómo utilizar el `PrincipalPermission` atributo en la GridView `RowUpdating` y `RowDeleting` controladores de eventos para prohibir la ejecución para que los usuarios no autorizados. Lo que debemos hacer es agregar el atributo apropiado de encima de cada definición de función:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

El atributo para el `RowUpdating` controlador de eventos indica que solo los usuarios de las funciones de los administradores o los supervisores pueden ejecutar el controlador de eventos, mientras que el atributo en el `RowDeleting` controlador de eventos limita la ejecución a los usuarios de los administradores rol.


> [!NOTE]
> El `PrincipalPermission` atributo se representa como una clase en el `System.Security.Permissions` espacio de nombres. Asegúrese de agregar un `Imports System.Security.Permissions` instrucción en la parte superior de su archivo de clase de código subyacente para importar este espacio de nombres.


Si, de algún modo, no administrador intenta ejecutar la `RowDeleting` controlador de eventos o si un intentos no Supervisor o sin privilegios de administrador para ejecutar el `RowUpdating` controlador de eventos, se producirá el runtime de .NET un `SecurityException`.


[![Si el contexto de seguridad no está autorizado para ejecutar el método, se produce una excepción de seguridad](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Figura 16**: si el contexto de seguridad no está autorizado para ejecutar el método, un `SecurityException` se produce ([haga clic aquí para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image48.png))


Además de las páginas ASP.NET, muchas aplicaciones también tienen una arquitectura que incluye varias capas, como la lógica de negocios y niveles de acceso a datos. Estos niveles normalmente se implementan como bibliotecas de clases y proporcionan clases y métodos para realizar la funcionalidad de relacionadas con datos y lógica empresarial. El `PrincipalPermission` atributo es útil para aplicar las reglas de autorización para estos niveles así.

Para obtener más información sobre el uso de la `PrincipalPermission` atributo para definir las reglas de autorización sobre clases y métodos, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog [agregar reglas de autorización a empresarial y el uso de las capas de datos `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumen

En este tutorial explicamos cómo especificar general y las reglas de autorización de concretos según los roles del usuario. ASP. Característica de autorización de URL de red permite que un desarrollador de páginas especificar qué identidades se permite o deniega el acceso a las páginas. Como hemos visto en la <a id="_msoanchor_10"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-vb.md) tutorial, autorización de dirección URL que se pueden aplicar reglas de forma por usuario. También puede aplicarse según una función por rol, tal y como hemos visto en el paso 1 de este tutorial.

De forma declarativa o mediante programación, se pueden aplicar reglas de autorización concretos. En el paso 2 analizamos utilizando la característica de RoleGroups del control LoginView para representar diferentes resultados basados en roles del usuario visitando. También explicamos formas de determinar mediante programación si un usuario pertenece a un rol específico y cómo ajustar la funcionalidad de la página según sea necesario.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Agregar reglas de autorización a Business y capas de datos con `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Examen de ASP.NET de 2.0 pertenencia, Roles y perfil: trabajar con Roles](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Lista de preguntas de seguridad para ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentación técnica de la `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial incluyen Suchi Banerjee y Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](assigning-roles-to-users-vb.md)
