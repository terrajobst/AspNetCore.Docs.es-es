---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Desbloquear y aprobar las cuentas de usuario (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial muestra cómo crear una página web para los administradores administrar bloqueada y aprueba los Estados de los usuarios. También verá cómo aprobar nuevos o de usuarios...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: f6bf76579898a8ff36b18380100ce4ab2e74fc8d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392720"
---
<a name="unlocking-and-approving-user-accounts-c"></a>Desbloquear y autorizar cuentas de usuario (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) o [descargar PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> Este tutorial muestra cómo crear una página web para los administradores administrar bloqueada y aprueba los Estados de los usuarios. También verá cómo aprobar los nuevos usuarios solo después de que ha comprobado su dirección de correo electrónico.


## <a name="introduction"></a>Introducción

Junto con un nombre de usuario, contraseña y correo electrónico, cada cuenta de usuario tiene dos campos de estado que determinen si el usuario puede iniciar sesión en el sitio: bloqueado y aprobado. Un usuario está bloqueado automáticamente si proporciona credenciales no válidas de un número especificado de veces durante un número especificado de minutos (la configuración predeterminada de bloquea un usuario tras 5 intentos de inicio de sesión no válido dentro de 10 minutos). El estado aprobado es útil en escenarios donde alguna acción debe transcurrir antes de un nuevo usuario puedo iniciar sesión en el sitio. Por ejemplo, un usuario deba en primer lugar, compruebe su dirección de correo electrónico o ser aprobados por un administrador antes de poder al inicio de sesión.

Dado que un bloqueo no aprobado o el usuario no puede iniciar sesión, es natural solo a preguntarse cómo se pueden restablecer estos Estados. ASP.NET no incluye ninguna funcionalidad integrada o controles Web para administrar los usuarios bloqueados y aprueban los Estados, en parte porque estas decisiones deben tratarse según el sitio por sitio. Algunos sitios pueden aprobar automáticamente todas las nuevas cuentas de usuario (el comportamiento predeterminado). Otros tienen un administrador apruebe las cuentas nuevas o no aprobar a los usuarios hasta que visitan un vínculo enviado a la dirección de correo electrónico que proporcionó cuando se registren. Del mismo modo, pueden bloquear algunos sitios a los usuarios hasta que un administrador restablece su estado, mientras que otros sitios envían un correo electrónico al usuario bloqueado con una dirección URL que pueden visitar para desbloquear su cuenta.

Este tutorial muestra cómo crear una página web para los administradores administrar bloqueada y aprueba los Estados de los usuarios. También verá cómo aprobar los nuevos usuarios solo después de que ha comprobado su dirección de correo electrónico.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Paso 1: Administración de los usuarios bloqueada y aprueba los Estados

En el <a id="Tutorial12"> </a> [ *crear una interfaz para seleccionar una cuenta de usuario de muchas* ](building-an-interface-to-select-one-user-account-from-many-cs.md) tutorial se crea una página que aparece cada cuenta de usuario de paginado, filtrado GridView. La cuadrícula enumera cada nombre de usuario y correo electrónico, sus Estados aprobados y bloqueados, si están conectados actualmente en línea y cualquier comentario sobre el usuario. Para administrar los usuarios aprobados y bloquea los Estados, podíamos hacer que esta cuadrícula editable. Para cambiar el estado de un usuario aprobado, el administrador podría encontrar primero la cuenta de usuario y, a continuación, edite la fila correspondiente de GridView, activando o desactivando la casilla de verificación aprobada. Como alternativa, podríamos administramos los Estados aprobados y bloqueados a través de una página ASP.NET independiente.

En este tutorial vamos a usar dos páginas ASP.NET: `ManageUsers.aspx` y `UserInformation.aspx`. La idea aquí es que `ManageUsers.aspx` enumera las cuentas de usuario en el sistema, mientras que `UserInformation.aspx` permite al administrador administrar los Estados aprobados y bloqueados para un usuario específico. Nuestra primera regla de negocio es aumentar el control GridView en `ManageUsers.aspx` para incluir un campo HYPERLINK, que se representa como una columna de vínculos. Queremos que cada vínculo para que apunte a `UserInformation.aspx?user=UserName`, donde *UserName* es el nombre del usuario para editar.

> [!NOTE]
> Si descargó el código para el <a id="Tutorial13"> </a> [ *recuperar y cambiar las contraseñas* ](recovering-and-changing-passwords-cs.md) tutorial quizás haya observado que la `ManageUsers.aspx` página ya contiene un conjunto de " Administrar"vínculos y `UserInformation.aspx` página proporciona una interfaz para cambiar la contraseña del usuario seleccionado. Decidido no replicar esa funcionalidad en el código asociado con este tutorial porque ha funcionado, evitando la API de pertenencia y trabajar directamente con la base de datos de SQL Server para cambiar la contraseña de un usuario. Este tutorial comienza desde cero con la `UserInformation.aspx` página.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Adición de "administrar" vínculos a la`UserAccounts`GridView

Abra el `ManageUsers.aspx` página y agregue un campo Hyperlink a la `UserAccounts` GridView. Establezca el campo de Hyperlink `Text` propiedad en "Administrar" y su `DataNavigateUrlFields` y `DataNavigateUrlFormatString` propiedades a `UserName` y "UserInformation.aspx?user={0}", respectivamente. Estos valores configuran el campo Hyperlink que todos los hipervínculos de mostrar el texto "Manage", pero cada vínculo que se pasa en los correspondientes *UserName* valor en la cadena de consulta.

Después de agregar el campo HYPERLINK en el control GridView, dedique un momento para ver el `ManageUsers.aspx` página a través de un explorador. Como se muestra en la figura 1, cada fila GridView ahora incluye un vínculo "Administrar". El vínculo "Administrar" para Bruce apunta a `UserInformation.aspx?user=Bruce`, mientras que el vínculo "Administrar" de Dave apunta a `UserInformation.aspx?user=Dave`.


[![Agrega el campo HYPERLINK un](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Figura 1**: el campo HYPERLINK agrega un vínculo "Administrar" para cada cuenta de usuario ([haga clic aquí para ver imagen en tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Se creará la interfaz de usuario y el código para el `UserInformation.aspx` página en un momento, pero primero vamos a hablar acerca de cómo cambiar mediante programación un usuario bloqueado y aprueba los Estados. El [ `MembershipUser` clase](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) tiene [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) y [ `IsApproved` propiedades](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). El `IsLockedOut` propiedad es de solo lectura. No hay ningún mecanismo para bloquear mediante programación un usuario; Para desbloquear un usuario, utilice el `MembershipUser` la clase [ `UnlockUser` método](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). El `IsApproved` propiedad es de lectura y escritura. Para guardar los cambios a esta propiedad, es necesario llamar a la `Membership` la clase [ `UpdateUser` método](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx), pasando el modificado `MembershipUser` objeto.

Dado que el `IsApproved` propiedad es de lectura y escritura, un control de casilla es probablemente el mejor elemento de interfaz de usuario para configurar esta propiedad. Sin embargo, una casilla de verificación no funcionará para el `IsLockedOut` propiedad porque no se puede bloquear un administrador de un usuario, solo puede desbloquear un usuario. Una interfaz de usuario adecuado para el `IsLockedOut` propiedad es un botón que, al hacer clic, desbloquea la cuenta de usuario. Este botón sólo debe habilitarse si el usuario está bloqueado.

### <a name="creating-theuserinformationaspxpage"></a>Crear el`UserInformation.aspx`página

Ahora estamos listos para implementar la interfaz de usuario `UserInformation.aspx`. Abra esta página y agregue los controles Web siguientes:

- Control de un hipervínculo que, al hacer clic, devuelve el Administrador de la `ManageUsers.aspx` página.
- Un control Web Label para mostrar el nombre del usuario seleccionado. Establecer esta etiqueta `ID` a `UserNameLabel` y borre su `Text` propiedad.
- Un control de casilla de verificación denominado `IsApproved`. Establezca su `AutoPostBack` propiedad `true`.
- Fecha del bloqueados por última vez un control de etiqueta para mostrar al usuario. Nombre de esta etiqueta `LastLockedOutDateLabel` y borre su `Text` propiedad.
- Un botón para desbloquear el usuario. Nombre de este botón `UnlockUserButton` y establezca su `Text` propiedad en "Desbloquear usuario".
- Un control de etiqueta para mostrar mensajes de estado, como "se ha actualizado el estado del usuario aprobado." Nombre de este control `StatusMessage`, desactive out su `Text` propiedad y establezca su `CssClass` propiedad `Important`. (El `Important` clase CSS que se define en el `Styles.css` el archivo de hoja de estilos; muestra el texto correspondiente en una fuente roja grande.)

Después de agregar estos controles, la vista de diseño en Visual Studio debe ser similar a la pantalla en la figura 2.


[![Crear la interfaz de usuario de UserInformation.aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Figura 2**: crear la interfaz de usuario de `UserInformation.aspx` ([haga clic aquí para ver imagen en tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Con la interfaz de usuario completa, la siguiente tarea consiste en establecer el `IsApproved` CheckBox y otros controles en función de información del usuario seleccionado. Crear un controlador de eventos de la página `Load` eventos y agregue el código siguiente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

El código anterior inicia asegurándose de que se trata de la primera visita a la página y no un postback subsiguientes. A continuación, lee el nombre de usuario pasado a través de la `user` campo querystring y recupera información sobre esa cuenta de usuario a través de la `Membership.GetUser(username)` método. Si se ha proporcionado ningún nombre de usuario a través de la cadena de consulta, o si no se encontró el usuario especificado, el administrador se envía a la `ManageUsers.aspx` página.

El `MembershipUser` del objeto `UserName` valor, a continuación, se muestra en el `UserNameLabel` y el `IsApproved` casilla se activa según el `IsApproved` valor de propiedad.

El `MembershipUser` del objeto [ `LastLockoutDate` propiedad](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) devuelve un `DateTime` bloqueado el valor que indica si el usuario por última vez. Si el usuario nunca se ha bloqueado, el valor devuelto depende del proveedor de pertenencia. Cuando se crea una nueva cuenta, el `SqlMembershipProvider` establece la `aspnet_Membership` la tabla `LastLockoutDate` campo `1754-01-01 12:00:00 AM`. El código anterior muestra una cadena vacía en el `LastLockoutDateLabel` si el `LastLockoutDate` propiedad se produce antes del año 2000; de lo contrario, parte de fecha de la `LastLockoutDate` propiedad se muestra en la etiqueta. El `UnlockUserButton'` s `Enabled` propiedad está establecida en el usuario bloquee el estado, lo que significa que este botón solo se habilita si el usuario está bloqueado.

Dedique un momento para probar la `UserInformation.aspx` página a través de un explorador. Por supuesto, deberá iniciar en `ManageUsers.aspx` y seleccione una cuenta de usuario para administrar. Tras la que llegan a `UserInformation.aspx`, tenga en cuenta que el `IsApproved` solo se activa la casilla si el usuario está aprobado. Si el usuario nunca se ha bloqueado, se muestra su último bloqueada por fecha. El botón Desbloquear usuario solo se habilita si el usuario está bloqueado actualmente. Activación o desactivación del `IsApproved` casilla de verificación o haciendo clic en el botón Desbloquear usuario produce un postback, pero no se realizan modificaciones en la cuenta de usuario porque aún no hemos crear controladores de eventos para estos eventos.

Vuelva a Visual Studio y cree controladores de eventos para el `IsApproved` de casilla de verificación `CheckedChanged` eventos y el `UnlockUser` del botón `Click` eventos. En el `CheckedChanged` controlador de eventos, establezca el usuario `IsApproved` propiedad a la `Checked` propiedad de la casilla de verificación y, a continuación, guarde los cambios a través de una llamada a `Membership.UpdateUser`. En el `Click` controlador de eventos, llamada simplemente la `MembershipUser` del objeto `UnlockUser` método. En ambos controladores de eventos, mostrar un mensaje adecuado en la `StatusMessage` etiqueta.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Probar la`UserInformation.aspx`página

Con estos controladores de eventos en su lugar, volver a visitar la página aprobadas y un usuario. Como se muestra en la figura 3, debería ver una breve mensaje en la página que indica que el usuario `IsApproved` propiedad se modificó correctamente.


[![Chris ha sido no aprobado](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Figura 3**: Chris ha sido no aprobado ([haga clic aquí para ver imagen en tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image9.png))


A continuación, cierre de sesión y vuelva a iniciar sesión como el usuario cuya cuenta era simplemente no aprobados. Dado que el usuario no está aprobado, no pueden iniciar sesión. De forma predeterminada, el control de inicio de sesión muestra el mismo mensaje si el usuario no puede iniciar sesión, independientemente del motivo. Sin embargo, en el <a id="Tutorial6"> </a> [ *validar usuario las credenciales en la pertenencia a usuario Store* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) tutorial analizamos mejora el control de inicio de sesión para mostrar un mensaje más apropiado. Como se muestra en la figura 4, Chris se muestra un mensaje que explique que él no se puede iniciar sesión porque su cuenta aún no está aprobada.


[![Chris no porque su cuenta de inicio es no aprobado](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Figura 4**: Chris no porque su cuenta de inicio es no aprobado ([haga clic aquí para ver imagen en tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Para probar la funcionalidad de bloqueada, intente iniciar sesión como un usuario aprobado, pero usar una contraseña incorrecta. Repita este proceso el número de veces hasta que se ha bloqueado la cuenta de usuario necesarios. El control de inicio de sesión también se actualiza para mostrar un personalizado del mensaje si intenta iniciar sesión desde una cuenta bloqueada. Sabe que una cuenta se ha bloqueado una vez que comienza a ver el mensaje siguiente en la página de inicio de sesión: "su cuenta se bloqueó debido a demasiados intentos de inicio de sesión no válido. Póngase en contacto con el administrador para que su cuenta desbloqueada."

Vuelva a la `ManageUsers.aspx` página y haga clic en el vínculo Administrar para el usuario bloqueado. Como se muestra en la figura 5, debería ver un valor en el `LastLockedOutDateLabel` debe habilitarse el botón Desbloquear usuario. Haga clic en el botón Desbloquear usuario para desbloquear la cuenta de usuario. Una vez que ha conseguido el usuario, podrá iniciar sesión de nuevo.


[![Dave se ha bloqueado fuera del sistema](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Figura 5**: Dave tiene ha bloqueado fuera del sistema ([haga clic aquí para ver imagen en tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Paso 2: Especificación de nuevos usuarios aprobados de estado

El estado aprobado es útil en escenarios donde desea que alguna acción que se realizará antes de un nuevo usuario puedo iniciar sesión y acceder a las características específicas del usuario del sitio. Por ejemplo, puede que esté ejecutando un sitio Web privado donde todas las páginas, excepto para las páginas de inicio de sesión y registro, son accesibles sólo a los usuarios autenticados. Pero ¿qué ocurre si un extraño llega a su sitio Web, detecta que la página de suscripción y crea una cuenta? Para evitar que esto suceda pudo mover la página de suscripción a un `Administration` carpeta y requieren que un administrador cree manualmente cada cuenta. Como alternativa, podría permitir que cualquier persona para la suscripción, pero prohibir el acceso al sitio hasta que un administrador apruebe la cuenta de usuario.

De forma predeterminada, el control CreateUserWizard aprueba las cuentas nuevas. Puede configurar este comportamiento mediante el control [ `DisableCreatedUser` propiedad](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Establezca esta propiedad en `true` no aprobar nuevas cuentas de usuario.

> [!NOTE]
> De forma predeterminada, el control CreateUserWizard conecta automáticamente a la nueva cuenta de usuario. Este comportamiento viene determinado por el control [ `LoginCreatedUser` propiedad](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Dado que no aprobadas a los usuarios no pueden iniciar sesión en el sitio, cuando `DisableCreatedUser` es `true` la nueva cuenta de usuario no está registrada en el sitio, independientemente del valor de la `LoginCreatedUser` propiedad.


Si va a crear mediante programación nuevas cuentas de usuario a través de la `Membership.CreateUser` método, para crear una cuenta de usuario no aprobadas se use una de las sobrecargas que aceptan el nuevo usuario `IsApproved` valor de propiedad como un parámetro de entrada.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Paso 3: Aprobar a los usuarios mediante la comprobación de su dirección de correo electrónico

Muchos sitios Web que admiten las cuentas de usuario no aprobar nuevos usuarios hasta que comprueban que proporciona al registrar la dirección de correo electrónico. Este proceso de comprobación normalmente se usa para frustrar los bots, los remitentes de spam y otras ne'er-do-wells ya que requiere una dirección de correo electrónico único, comprobado y agrega un paso adicional en el proceso de suscripción. Con este modelo, cuando se suscribe a un nuevo usuario se envía un mensaje de correo electrónico que incluye un vínculo a una página de comprobación. Visitando el vínculo el usuario ha demostrado que recibieron el correo y, por lo tanto, que el correo electrónico dirección siempre es válido. La página de comprobación es responsable de aprobar el usuario. Esto puede ocurrir automáticamente, aprobar, por tanto, cualquier usuario que llega a esta página, o solo después de que el usuario proporciona información adicional, como un [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Para dar cabida a este flujo de trabajo, se debe actualizar la página de creación de cuenta para que los nuevos usuarios no están aprobados. Abra el `EnhancedCreateUserWizard.aspx` página en el `Membership` carpeta y establezca el control CreateUserWizard `DisableCreatedUser` propiedad `true`.

A continuación, se debe configurar el control CreateUserWizard para enviar un correo electrónico al nuevo usuario con instrucciones sobre cómo comprobar su cuenta. En concreto, se incluirá un vínculo del correo electrónico para el `Verification.aspx` página (que aún no hemos crear), pasando el nuevo usuario `UserId` a través de la cadena de consulta. El `Verification.aspx` página Buscar al usuario especificado y marcarlos como aprobado.

### <a name="sending-a-verification-email-to-new-users"></a>Enviar un correo electrónico de comprobación a nuevos usuarios

Para enviar un correo electrónico desde el control CreateUserWizard, configure su `MailDefinition` propiedad adecuadamente. Como se describe en el <a id="Tutorial13"> </a> [tutorial anterior](recovering-and-changing-passwords-cs.md), los controles ChangePassword y PasswordRecovery incluyen un [ `MailDefinition` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) que funciona de la misma manera que del control CreateUserWizard.

> [!NOTE]
> Para usar el `MailDefinition` opciones de propiedad que necesita para especificar la entrega de correo electrónico en `Web.config`. Para obtener más información, consulte [enviar correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Empiece por crear una nueva plantilla de correo electrónico denominada `CreateUserWizard.txt` en el `EmailTemplates` carpeta. Use el siguiente texto para la plantilla:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Establecer el `MailDefinition'` s `BodyFileName` propiedad en "~ / EmailTemplates/CreateUserWizard.txt" y su `Subject` propiedad en "Bienvenido a Mi sitio Web. "Activar su cuenta."

Tenga en cuenta que el `CreateUserWizard.txt` plantilla de correo electrónico incluye un `<%VerificationUrl%>` marcador de posición. Aquí es donde la dirección URL para el `Verification.aspx` se colocará la página. CreateUserWizard reemplaza automáticamente el `<%UserName%>` y `<%Password%>` marcadores de posición con de la nueva cuenta nombre de usuario y contraseña, pero no hay ningún integrado `<%VerificationUrl%>` marcador de posición. Es necesario reemplazarlo manualmente con la dirección URL de comprobación adecuados.

Para ello, cree un controlador de eventos para el CreateUserWizard [ `SendingMail` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) y agregue el código siguiente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

El `SendingMail` evento se desencadena después de la `CreatedUser` eventos, lo que significa que en el momento en el nuevo usuario ejecuta el controlador de eventos anterior ya se han creado cuenta. Podemos acceder al nuevo usuario `UserId` valor mediante una llamada a la `Membership.GetUser` método, pasando el `UserName` especificado en el control CreateUserWizard. A continuación, se forma a la dirección URL de comprobación. La instrucción `Request.Url.GetLeftPart(UriPartial.Authority)` devuelve el `http://yourserver.com` parte de la dirección URL; `Request.ApplicationPath` devuelve ruta de acceso donde se ha modificado la aplicación. La comprobación de la dirección URL, a continuación, se define como `Verification.aspx?ID=userId`. Estas dos cadenas, a continuación, se concatenan para formar la dirección URL completa. Por último, el cuerpo del mensaje de correo electrónico (`e.Message.Body`) tiene todas las apariciones de `<%VerificationUrl%>` reemplazado por la dirección URL completa.

El efecto neto es que los usuarios nuevos están aprobados, lo que significa que no se puede iniciar sesión en el sitio. Además, se envían automáticamente un correo electrónico con un vínculo a la dirección URL de comprobación (consulte la figura 6).


[![El nuevo usuario recibe un correo electrónico con un vínculo a la dirección URL de comprobación](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Figura 6**: el nuevo usuario recibe un correo electrónico con un vínculo a la dirección URL de comprobación ([haga clic aquí para ver imagen en tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> Paso de CreateUserWizard predeterminado del control CreateUserWizard muestra un mensaje que informa al usuario su cuenta se ha creado y muestra un botón Continuar. Al hacer clic en esto lleva al usuario a la dirección URL especificada por el control `ContinueDestinationPageUrl` propiedad. El control CreateUserWizard en `EnhancedCreateUserWizard.aspx` está configurado para enviar a los nuevos usuarios el `~/Membership/AdditionalUserInfo.aspx`, que solicita al usuario para su ciudad natal, la dirección URL de la página principal y la firma. Dado que esta información solo se puede agregar por usuarios que iniciaron sesión, tiene sentido actualizar esta propiedad para enviar a los usuarios volver a página principal de la carpeta del sitio (`~/Default.aspx`). Además, la `EnhancedCreateUserWizard.aspx` página o el paso CreateUserWizard debe ampliarse para informar al usuario que se envió un correo electrónico de comprobación y su cuenta no se activarán hasta que siga las instrucciones de este correo electrónico. Dejar estas modificaciones como un ejercicio para el lector.


### <a name="creating-the-verification-page"></a>Creación de la página de comprobación

Nuestra última tarea consiste en crear el `Verification.aspx` página. Agregue esta página a la carpeta raíz, lo asocia a la `Site.master` página maestra. Como hemos hecho con la mayoría de las páginas de contenido anteriores agregadas al sitio, quite el control de contenido que hace referencia a la `LoginContent` ContentPlaceHolder para que la página de contenido usa la página principal de contenido predeterminada.

Agregar un control Web de la etiqueta a la `Verification.aspx` , establezca su `ID` a `StatusMessage` y borre su propiedad text. A continuación, cree el `Page_Load` controlador de eventos y agregue el código siguiente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

La mayor parte del código anterior comprueba que el `UserId` proporcionado a través de la cadena de consulta existe, que es válido `Guid` valor y que hace referencia a una cuenta de usuario. Si se superan todas estas comprobaciones, se aprueba la cuenta de usuario; en caso contrario, se muestra un mensaje de estado adecuado.

La figura 7 muestra el `Verification.aspx` página cuando visita a través de un explorador.


[![La nueva cuenta de usuario es aprobado ahora](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Figura 7**: cuenta de usuario la nueva es aprobado ahora ([haga clic aquí para ver imagen en tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Resumen

Todas las cuentas de usuario de pertenencia tienen dos Estados que determinan si el usuario puede iniciar sesión en el sitio: `IsLockedOut` y `IsApproved`. Dos de estas propiedades deben ser `true` para el usuario al inicio de sesión.

El usuario bloqueado del estado se utiliza como medida de seguridad para reducir la probabilidad de que un hacker dividir en un sitio a través de métodos de fuerza bruta. En concreto, un usuario está bloqueado si hay un cierto número de intentos de inicio de sesión no válido dentro de un determinado período de tiempo. Estos límites son configurables a través de la configuración del proveedor de pertenencia en `Web.config`.

El estado aprobado se utiliza normalmente como un medio para impedir que los nuevos usuarios iniciar sesión hasta que haya llegado alguna acción. Quizás el sitio requiere que las nuevas cuentas en primer lugar deben ser aprobadas por el administrador o, como hemos visto en el paso 3, mediante la comprobación de su dirección de correo electrónico.

Feliz programación.

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](recovering-and-changing-passwords-cs.md)
> [Siguiente](building-an-interface-to-select-one-user-account-from-many-vb.md)
