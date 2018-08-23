---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: Recuperar y cambiar las contraseñas (C#) | Microsoft Docs
author: rick-anderson
description: ASP.NET incluye dos controles Web de ayudar a recuperar y cambiar las contraseñas. El control PasswordRecovery permite a un visitante recuperar a su pa perdido...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: c04ed8ae18a3739f5519e30dea7768b8f6c7c7ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837678"
---
<a name="recovering-and-changing-passwords-c"></a>Recuperar y cambiar las contraseñas (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) o [descargar PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> ASP.NET incluye dos controles Web de ayudar a recuperar y cambiar las contraseñas. El control PasswordRecovery permite a un visitante recuperar su contraseña perdida. El control ChangePassword permite al usuario que actualice su contraseña. Al igual que otros controles Web relacionados con el inicio de sesión que hemos visto a lo largo de esta serie de tutoriales, la PasswordRecovery y ChangePassword de los controles con el marco de pertenencia en segundo plano para restablecer o modificar las contraseñas de usuario.


## <a name="introduction"></a>Introducción

Entre los sitios Web para mi bank, compañía, compañía telefónica, cuentas de correo electrónico y los portales web personalizada, I, al igual que la mayoría de los usuarios tener docenas de distintas contraseñas para recordar. Con tantas credenciales memorizar hoy en día, no es raro que las personas olvidado su contraseña. Para resolverlo, deben incluir un usuario recuperar su contraseña de una manera de sitios Web que ofrecen las cuentas de usuario. Este proceso conlleva normalmente genera una nueva contraseña aleatoria y enviarla a la dirección de correo electrónico del usuario en el archivo. Después de recibir su nueva contraseña la mayoría de los usuarios volver al sitio y cambiar su contraseña desde el uno generado aleatoriamente a uno más fáciles de recordar.

ASP.NET incluye dos controles Web de ayudar a recuperar y cambiar las contraseñas. El control PasswordRecovery permite a un visitante recuperar su contraseña perdida. El control ChangePassword permite al usuario que actualice su contraseña. Al igual que otros controles Web relacionados con el inicio de sesión que hemos visto a lo largo de esta serie de tutoriales, la PasswordRecovery y ChangePassword de los controles con el marco de pertenencia en segundo plano para restablecer o modificar las contraseñas de usuario.

En este tutorial, examinaremos el uso de estos dos controles. También veremos cómo cambiar mediante programación y restablecer la contraseña de un usuario a través de la `MembershipUser` la clase `ChangePassword` y `ResetPassword` métodos.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Paso 1: Le ayuda a los usuarios recuperar pierden las contraseñas

Todos los sitios Web que admiten las cuentas de usuario deben proporcionar a los usuarios con algún mecanismo para recuperar sus contraseñas olvidadas. La buena noticia es que la implementación de esta funcionalidad en ASP.NET es muy fácil gracias al control PasswordRecovery Web. El control PasswordRecovery representa una interfaz que solicita al usuario su nombre de usuario y, si es necesario, la respuesta a su pregunta de seguridad. A continuación, envía un correo electrónico al usuario su contraseña.

> [!NOTE]
> Dado que los mensajes de correo electrónico se transmiten a través de la red en texto sin formato intervienen los riesgos de seguridad con el envío de contraseña de un usuario por correo electrónico.


El control PasswordRecovery consta de tres vistas:

- **Nombre de usuario** -solicita el visitante para su nombre de usuario. Se trata de la vista inicial.
- **Pregunta**-muestra la pregunta de seguridad y el nombre de usuario del usuario como texto, junto con un cuadro de texto para el usuario a escribir la respuesta a su pregunta de seguridad.
- **Éxito**-muestra un mensaje que informa al usuario que se ha enviado su contraseña.

Muestran las vistas y las acciones realizadas por el control PasswordRecovery dependen de las siguientes opciones de configuración de pertenencia:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

El marco de pertenencia `RequiresQuestionAndAnswer` valor indica si los usuarios deben especificar una pregunta de seguridad y la respuesta al registrarse para una cuenta. Como se explicó en la <a id="_msoanchor_1"> </a> [ *crear cuentas de usuario* ](../membership/creating-user-accounts-cs.md) tutorial, if `RequiresQuestionAndAnswer` es True (valor predeterminado), entonces la interfaz del control CreateUserWizard incluye el cuadro de texto controles para el nuevo usuario pregunta de seguridad y respuesta; Si `RequiresQuestionAndAnswer` es False, se recopila ninguna información de este tipo. De forma similar, si `RequiresQuestionAndAnswer` es True, entonces el control PasswordRecovery muestra la pregunta ver después de que el usuario ha escrito su nombre de usuario, ya que es recuperar la contraseña solo si el usuario escribe la respuesta de seguridad correcta. Si `RequiresQuestionAndAnswer` es False, sin embargo, se mueve el control PasswordRecovery directamente desde la vista de nombre de usuario a la vista correcto.

Después de que el usuario ha proporcionado su nombre de usuario - o su respuesta de nombre de usuario y la seguridad, si `RequiresQuestionAndAnswer` es True: la PasswordRecovery envía por correo electrónico al usuario su contraseña. Si el `EnablePasswordRetrieval` opción está establecida en True y, después, el usuario se envían por correo electrónico su contraseña actual. Si se establece en False y `EnablePasswordReset` está establecida en True, el control PasswordRecovery genera una nueva contraseña aleatoria para el usuario y envía por correo electrónico de esta nueva contraseña a ellos. Si ambos `EnablePasswordRetrieval` y `EnablePasswordReset` son False, el control PasswordRecovery produce una excepción.

> [!NOTE]
> Recuerde que el `SqlMembershipProvider` almacena las contraseñas de usuario en uno de los tres formatos: Clear, Hashed (valor predeterminado) o cifrado. El mecanismo de almacenamiento utilizado depende de las opciones de configuración de pertenencia; la aplicación de demostración usa el formato de la contraseña con hash. Cuando se usa el formato de la contraseña Hashed el `EnablePasswordRetrieval` opción debe establecerse en False porque el sistema no puede determinar la contraseña del usuario real de la versión con hash almacenada en la base de datos.


Figura 1 ilustra cómo el PasswordRecovery interfaz y el comportamiento se ve afectada por la configuración de pertenencia.


[![El RequiresQuestionAndAnswer y EnablePasswordRetrieval EnablePasswordReset influir en apariencia y el comportamiento del control PasswordRecovery](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Figura 1**: el `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, y `EnablePasswordReset` influir en la apariencia y el comportamiento del control PasswordRecovery ([haga clic aquí para ver imagen en tamaño completo](recovering-and-changing-passwords-cs/_static/image3.png))


> [!NOTE]
> En el <a id="_msoanchor_2"> </a> [ *crear el esquema de pertenencia en SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) tutorial hemos configurado el proveedor de pertenencia estableciendo `RequiresQuestionAndAnswer` en True, `EnablePasswordRetrieval` a Es False, y `EnablePasswordReset` en True.


### <a name="using-the-passwordrecovery-control"></a>Uso del Control PasswordRecovery

Veamos cómo usar el control PasswordRecovery en una página ASP.NET. Abra `RecoverPassword.aspx` y arrastre y coloque un control PasswordRecovery desde el cuadro de herramientas hasta el diseñador; establecer su `ID` a `RecoverPwd`. Igual que los controles de inicio de sesión y CreateUserWizard Web, las vistas del control PasswordRecovery presentar una interfaz enriquecida compuesta que incluye las etiquetas, cuadros de texto, botones y los controles de validación. Puede personalizar la apariencia de las vistas a través de las propiedades de estilo del control o convirtiendo las vistas en las plantillas. Dejar como un ejercicio para el lector interesado.

Cuando un usuario visita esta página introducirá el nombre de usuario y haga clic en el botón Enviar. Dado que hemos configurado la `RequiresQuestionAndAnswer` propiedad en True en nuestras opciones de configuración de pertenencia, el PasswordRecovery controlar will, a continuación, mostrar la vista de pregunta. Después de que el usuario escribe su respuesta de seguridad correcta y haga clic en Submit, el control PasswordRecovery actualizar la contraseña del usuario a uno generado aleatoriamente y esta contraseña a la dirección de correo electrónico en el archivo de correo electrónico. Todo esto era posible sin que tengamos que escribir una sola línea de código.

Antes de probar esta página, hay un último elemento de configuración que tienden a: se debe especificar la configuración de entrega de correo electrónico en `Web.config`. El control PasswordRecovery se basa en esta configuración para enviar el correo electrónico.

Se especifica la configuración de entrega de correo electrónico a través de la [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx)del [ `<mailSettings>` elemento](https://msdn.microsoft.com/library/w355a94k.aspx). Use la [ `<smtp>` elemento](https://msdn.microsoft.com/library/ms164240.aspx) para indicar el método de entrega y el valor predeterminado de dirección. El siguiente marcado establece la configuración de correo electrónico para usar un servidor SMTP de red denominado `smtp.example.com` en el puerto 25 y con las credenciales de usuario y contraseña de usuario y la contraseña.

> [!NOTE]
> `<system.net>` es un elemento secundario de la raíz `<configuration>` elemento y un elemento relacionado de `<system.web>`. Por lo tanto, no coloque la `<system.net>` elemento dentro de la `<system.web>` elemento; en su lugar, se coloca en el mismo nivel.


[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Además de utilizar un servidor SMTP en la red, o bien puede especificar un directorio de recogida donde se deben depositar los mensajes de correo electrónico se envíen.

Una vez haya configurado la configuración de SMTP, visite la `RecoverPassword.aspx` página a través de un explorador. En primer lugar pruebe a escribir un nombre de usuario que no existe en el almacén del usuario. Como se muestra en la figura 2, el control PasswordRecovery muestra un mensaje que indica que no se accede a la información de usuario. Se puede personalizar el texto del mensaje a través del control [ `UserNameFailureText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Se muestra un mensaje de Error si se escribe un nombre de usuario no válido](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Figura 2**: se muestra un mensaje de Error si se escribe un nombre de usuario no válido ([haga clic aquí para ver imagen en tamaño completo](recovering-and-changing-passwords-cs/_static/image6.png))


Ahora, escriba un nombre de usuario. Use el nombre de usuario de una cuenta en el sistema con una dirección de correo electrónico que puede acceder y responder a cuya seguridad se conoce. Después de escribir el nombre de usuario y hacer clic en enviar, el control PasswordRecovery muestra su vista de pregunta. Como con la vista de nombre de usuario, si escribe una incorrecta responda el control PasswordRecovery muestra un mensaje de error (consulte la figura 3). Use la [ `QuestionFailureText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) para personalizar este mensaje de error.


[![Se muestra un mensaje de Error si el usuario escribe una respuesta de seguridad no válido](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Figura 3**: se muestra un mensaje de Error si el usuario escribe una respuesta de seguridad no válido ([haga clic aquí para ver imagen en tamaño completo](recovering-and-changing-passwords-cs/_static/image9.png))


Por último, escriba la respuesta de seguridad correcta y haga clic en enviar. En segundo plano, el control PasswordRecovery genera una contraseña aleatoria, lo asigna a la cuenta de usuario, envía un correo electrónico que informa al usuario de su nueva contraseña (consulte la figura 4) y, a continuación, muestra la vista correcto.


[![El usuario se envía un correo electrónico con su nueva contraseña](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Figura 4**: el usuario se envía un correo electrónico con su nueva contraseña ([haga clic aquí para ver imagen en tamaño completo](recovering-and-changing-passwords-cs/_static/image12.png))


### <a name="customizing-the-email"></a>Personalizar el correo electrónico

El correo electrónico predeterminado enviado por el control PasswordRecovery es bastante aburrido (consulte la figura 4). El mensaje se envía desde la cuenta especificada en el `<smtp>` del elemento `from` atributo con el asunto de contraseña y el cuerpo de texto sin formato:

Regrese al sitio e inicie sesión con la siguiente información.

Nombre de usuario: *nombre de usuario*

Contraseña: *contraseña*

Este mensaje se puede personalizar mediante programación a través de un controlador de eventos para el control PasswordRecovery [ `SendingMail` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), o mediante declaración a través del [ `MailDefinition` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Vamos a examinar ambas opciones.

El `SendingMail` evento se desencadena justo antes de que el mensaje de correo electrónico se envía y se nuestra última oportunidad para ajustar el mensaje de correo electrónico mediante programación. Cuando se genera este evento, el controlador de eventos se pasa un objeto de tipo [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), cuya `Message` propiedad contiene una referencia al correo electrónico que va a enviar.

Crear un controlador de eventos para el `SendingMail` eventos y agregue el código siguiente, que agrega mediante programación `webmaster@example.com` a la lista CC.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

El mensaje de correo electrónico también puede configurarse a través de medios declarativos. El PasswordRecovery `MailDefinition` propiedad es un objeto de tipo [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). El `MailDefinition` clase ofrece un sinfín de propiedades relacionadas con el correo electrónico, incluidos `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`y otros. Para comenzar, establezca el [ `Subject` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) a algo más descriptivo a la que se usa de forma predeterminada (contraseña), como se ha restablecido su contraseña...

Para personalizar el cuerpo del mensaje de correo electrónico que es necesario crear un archivo de plantilla de correo electrónico independiente que contiene el contenido del cuerpo. Empiece por crear una nueva carpeta en el sitio Web denominado `EmailTemplates`. A continuación, agregue un nuevo archivo de texto en esta carpeta denominada `PasswordRecovery.txt` y agregue el siguiente contenido:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Tenga en cuenta el uso de los marcadores de posición `<%UserName%>` y `<%Password%>`. El control PasswordRecovery reemplaza automáticamente estos dos marcadores de posición con nombre de usuario y una contraseña recuperada antes de enviar el correo electrónico del usuario.

Por último, elija el `MailDefinition`del [ `BodyFileName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) a la plantilla de correo electrónico que acabamos de crear (`~/EmailTemplates/PasswordRecovery.txt`).

Después de realizar estos cambios a consultar el `RecoverPassword.aspx` página y escriba su nombre de usuario y seguridad la respuesta. Recibirá debe un correo electrónico que parezca más en la figura 5. Tenga en cuenta que `webmaster@example.com` ha sido CC debería y que se han actualizado el asunto y cuerpo.


[![Se han actualizado el asunto y cuerpo de la lista CC](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Figura 5**: el asunto, el cuerpo y se han actualizado CC lista ([haga clic aquí para ver imagen en tamaño completo](recovering-and-changing-passwords-cs/_static/image15.png))


Enviar un correo electrónico con formato HTML establecer [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) en True (el valor predeterminado es False) y actualización de la plantilla de correo electrónico para incluir HTML.

El `MailDefinition` propiedad no es única en la clase PasswordRecovery. Como veremos en el paso 2, el control ChangePassword también ofrece un `MailDefinition` propiedad. Además, el control CreateUserWizard incluye este tipo de propiedad que se pueden configurar para enviar automáticamente un mensaje de correo electrónico de bienvenida a nuevos usuarios.

> [!NOTE]
> Actualmente hay no hay vínculos en el panel de navegación izquierdo para llegar a la `RecoverPassword.aspx` página. Un usuario solo estaría interesado en visitar esta página si ella no pudo iniciar sesión en el sitio. Por lo tanto, actualizar la `Login.aspx` página para incluir un vínculo a la `RecoverPassword.aspx` página.


### <a name="programmatically-resetting-a-users-password"></a>Mediante programación al restablecer una contraseña de usuario

Cuando el restablecimiento de contraseña de un usuario la PasswordRecovery control llama a la `MembershipUser` del objeto [ `ResetPassword` método](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Este método tiene dos sobrecargas:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -restablece una contraseña de usuario. Utilice esta sobrecarga si `RequiresQuestionAndAnswer` es False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -Restablece solo si de contraseña de un usuario proporcionado *securityAnswer* es correcta. Utilice esta sobrecarga si `RequiresQuestionAndAnswer` es True.

Ambas sobrecargas devuelven la nueva contraseña generada de forma aleatoria.

Al igual que con los otros métodos en el marco de pertenencia, el `ResetPassword` método delega en el proveedor configurado. El `SqlMembershipProvider` invoca el `aspnet_Membership_ResetPassword` procedimiento almacenado, pasando en el nombre de usuario, la nueva contraseña y la respuesta de contraseña proporcionada, entre otros campos. El procedimiento almacenado garantiza que la respuesta de contraseña coincide con y, a continuación, actualiza la contraseña del usuario.

Un par de notas de la implementación de bajo nivel:

- Un usuario bloqueado no puede restablecer su contraseña. Sin embargo, es posible que un usuario no aprobado. Se tratará la bloqueado y aprobado con más detalle en los Estados del <a id="_msoanchor_3"> </a> [ *Unlocking y aprobación de usuario* ](unlocking-and-approving-user-accounts-cs.md) tutorial de cuentas.
- Si la respuesta de contraseña es incorrecta, se incrementa la cantidad de intentos de respuesta de contraseña del usuario. Si se produce un número especificado de intentos de respuesta de seguridad no válido dentro de un período de tiempo especificado, el usuario está bloqueado.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Se generan una palabra en cómo las contraseñas aleatorias

Las contraseñas generado aleatoriamente que se muestra en los mensajes de correo electrónico en las figuras 4 y 5 se crean mediante la clase de pertenencia [ `GeneratePassword` método](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Este método acepta dos parámetros de entrada entero - *longitud* y *numberOfNonAlphanumericCharacters* - y devuelve una cadena al menos *longitud* largo con los caracteres en menos *numberOfNonAlphanumericCharacters* número de caracteres no alfanuméricos. Cuando este método se llama desde dentro de las clases de pertenencia o los controles Web relacionados con el inicio de sesión, los valores para estos dos parámetros se determinan mediante la configuración de pertenencia `MinRequiredPasswordLength` y `MinRequiredNonalphanumericCharacters` propiedades, que se establecen en 7 y 1, respectivamente.

El `GeneratePassword` método utiliza un generador de números aleatorios criptográficamente seguro para asegurarse de que no hay ninguna diferencia en los caracteres aleatorios se seleccionan. Además, `GeneratePassword` es `public`, lo que significa que se puede utilizar directamente desde la aplicación de ASP.NET si tiene que generar cadenas aleatorias o contraseñas.

> [!NOTE]
> El `SqlMembershipProvider` clase siempre genera una contraseña aleatoria al menos 14 caracteres, por lo que si `MinRequiredPasswordLength` es inferior a 14, a continuación, se omite su valor.


## <a name="step-2-changing-passwords"></a>Paso 2: Cambiar las contraseñas

Son difíciles de recordar las contraseñas generada aleatoriamente. Tenga en cuenta la contraseña que se muestra en la figura 4: `WWGUZv(f2yM:Bd`. Pruebe confirmar en memoria. Huelga decir, cuando un usuario se envía una contraseña generada de forma aleatoria de este tipo, ella quiera cambiar la contraseña a algo más fácil de recordar.

Utilice el control ChangePassword para crear una interfaz para un usuario que cambie su contraseña. Muy similar al control PasswordRecovery, el control ChangePassword consta de dos vistas: cambiar la contraseña y el éxito. La vista Cambiar contraseña solicita al usuario sus contraseñas antiguas y nuevas. Tras proporcionar la contraseña antigua correcta y una nueva contraseña que cumpla los requisitos de caracteres no alfanuméricos y la longitud mínima, el control ChangePassword actualiza la contraseña del usuario y muestra la vista correcto.

> [!NOTE]
> El control ChangePassword modifica la contraseña del usuario mediante la invocación del `MembershipUser` del objeto [ `ChangePassword` método](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). El método ChangePassword acepta dos `string` de entrada de los parámetros - *oldPassword* y *newPassword*- y actualiza la cuenta de usuario con el *newPassword*, Suponiendo que el proporcionado *oldPassword* es correcta.


Abra el `ChangePassword.aspx` página y agregue un control ChangePassword a la página, asígnele el nombre `ChangePwd`. En este momento, la vista de diseño debe mostrar el cambio de contraseña ver (consulte la figura 6). Al igual que con el control PasswordRecovery, puede alternar entre las vistas a través de la etiqueta inteligente del control. Además, las apariencias de estas vistas son personalizables a través de las propiedades de estilo diversas o mediante su conversión a una plantilla.


[![Agregar un Control ChangePassword a la página](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Figura 6**: agregar un ChangePassword Control a la página ([haga clic aquí para ver imagen en tamaño completo](recovering-and-changing-passwords-cs/_static/image18.png))


El control ChangePassword puede actualizar la contraseña del usuario con sesión iniciada actualmente *o* la contraseña de otro usuario especificado. Como se muestra en la figura 6, la vista Cambiar contraseña predeterminada representa sólo tres controles de cuadro de texto: uno para la contraseña anterior y dos para la nueva contraseña. Esta interfaz predeterminada se utiliza para actualizar la contraseña del usuario ha iniciado sesión actualmente.

Para usar el control ChangePassword para actualizar la contraseña de otro usuario, establezca el control [ `DisplayUserName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) en True. Si lo hace, agrega un cuarto cuadro de texto a la página, que se solicite el nombre de usuario del usuario cuya contraseña para cambiar.

Establecer `DisplayUserName` a True es útil si desea permitir que los usuarios registrados a cambiar su contraseña sin tener que iniciar sesión. Personalmente, creo que no hay nada de malo obligar al usuario al inicio de sesión antes de permitir que ella cambiar su contraseña. Por lo tanto, deje `DisplayUserName` establecida en False (su valor predeterminado). Para tomar esta decisión, sin embargo, nos básicamente estamos salvo los usuarios anónimos de llegar a esta página. Actualizar reglas de autorización de dirección URL del sitio con el fin de impedir que los usuarios anónimos visite `ChangePassword.aspx`. Si necesita actualizar la memoria en la sintaxis de regla de autorización de URL, vuelva a consultar el <a id="_msoanchor_4"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-cs.md) tutorial.

> [!NOTE]
> Puede parecer que el `DisplayUserName` propiedad resulta útil para permitir que los administradores cambiar las contraseñas de otros usuarios. Sin embargo, incluso cuando `DisplayUserName` está establecido en True, la contraseña antigua correcta debe sabe y escribirse. Hablaremos sobre las técnicas para permitir que los administradores cambiar las contraseñas de usuario en el paso 3.


Visite el `ChangePassword.aspx` página a través de un explorador y cambie su contraseña. Tenga en cuenta que se muestra un mensaje de error si escribe una nueva contraseña que no cumple la longitud de contraseña y los requisitos de caracteres no alfanuméricos especificados en la configuración de pertenencia (consulte la figura 7).


[![Agregar un Control ChangePassword a la página](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Figura 7**: agregar un ChangePassword Control a la página ([haga clic aquí para ver imagen en tamaño completo](recovering-and-changing-passwords-cs/_static/image21.png))


Después de escribir la contraseña antigua correcta y una contraseña nueva válida, el inicio de sesión del usuario de contraseña se cambia y muestra la vista correcto.

### <a name="sending-a-confirmation-email"></a>Enviar un correo electrónico de confirmación

De forma predeterminada, el control ChangePassword no envía un mensaje de correo electrónico al usuario cuya contraseña se actualizó recientemente. Si desea enviar un correo electrónico, basta con configurar el control `MailDefinition` propiedad. Vamos a configurar el control ChangePassword para que se envíe un correo electrónico con formato HTML que contiene la nueva contraseña al usuario.

Empiece por crear un nuevo archivo en el `EmailTemplates` carpeta denominada `ChangePassword.htm`. Agregue el siguiente marcado:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

A continuación, establezca el control ChangePassword `MailDefinition` la propiedad `BodyFileName`, `IsBodyHtml`, y `Subject` propiedades en ~ / EmailTemplates/ChangePassword.htm, True y la contraseña ha cambiado!, respectivamente.

Después de realizar estos cambios, volver a visitar la página y volver a cambiar su contraseña. Esta vez, el control ChangePassword envía un correo electrónico personalizado, con formato HTML a la dirección de correo electrónico del usuario en el archivo (consulte la figura 8).


[![Un mensaje de correo electrónico que informa de que ha cambiado la contraseña del usuario que sus](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Figura 8**: un mensaje de correo electrónico informa de que ha cambiado la contraseña del usuario que sus ([haga clic aquí para ver imagen en tamaño completo](recovering-and-changing-passwords-cs/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Paso 3: Permitir que los administradores cambiar las contraseñas de usuario

Una característica común en aplicaciones que admiten las cuentas de usuario es la capacidad para un usuario administrativo cambiar las contraseñas de otros usuarios. A veces, esta funcionalidad es necesaria porque el sistema no tiene la posibilidad de que los usuarios cambien sus contraseñas. En tal caso, la única forma de un usuario para recuperar su contraseña olvidada sería el administrador les asigne una nueva contraseña. Con los controles PasswordRecovery y ChangePassword, sin embargo, los usuarios administrativos necesitan no ocupado a sí mismos con el cambio de contraseñas de usuarios, como los usuarios son capaces de hacer este a sí mismos.

Pero ¿qué ocurre si el cliente insiste en que los usuarios administrativos puedan cambiar las contraseñas de otros usuarios? Por desgracia, al agregar esta funcionalidad puede ser un poco de trabajo. Para cambiar la contraseña de un usuario, se debe proporcionar la contraseña antigua y nueva a la `MembershipUser` del objeto `ChangePassword` método, pero un administrador no debería tener que conocer la contraseña de un usuario para modificarlo.

Una solución alternativa es primero restablecer la contraseña del usuario y, a continuación, cámbielo a la nueva contraseña mediante código similar al siguiente:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Este código se inicia mediante la recuperación de información sobre *username*, que es el usuario cuya contraseña desea cambiar el administrador. A continuación, el `ResetPassword` se invoca al método, que asigna y el usuario una nueva contraseña aleatoria. Esta contraseña generada de forma aleatoria es devuelto por el método y se almacena en la variable `resetPwd`. Ahora que sabemos que la contraseña del usuario, se puede cambiar mediante una llamada a `ChangePassword`.

El problema es que este código sólo funciona si se establece la configuración del sistema de pertenencia que `RequiresQuestionAndAnswer` es False. Si `RequiresQuestionAndAnswer` es True, como sucede con nuestra aplicación, el `ResetPassword` método debe pasarse a la respuesta de seguridad, en caso contrario producirá una excepción.

Si el marco de pertenencia está configurado para solicitar una pregunta de seguridad y respuesta, y aún su cliente insiste en que los administradores podrán cambiar las contraseñas de usuario, tiene tres opciones:

- Iniciar las manos en el aire y saber que el cliente que esto es solo una cosa que no se puede realizar.
- Establecer `RequiresQuestionAndAnswer` en False. Esto da como resultado una aplicación menos segura. Imagine que un usuario viles haya obtenido acceso a la Bandeja de entrada de correo electrónico a otro usuario. Quizás el usuario en peligro ha dejado sus puestos de trabajo para ir a comer y no bloquea su estación de trabajo, o quizás tiene acceso a su correo electrónico desde un terminal público y no cierre la sesión. En cualquier caso, el usuario viles puede visitar el `RecoverPassword.aspx` página y escriba el nombre de usuario. El sistema, a continuación, enviaremos por correo electrónico la contraseña recuperada sin pedir la respuesta de seguridad.
- Omitir la capa de abstracción creada por el marco de pertenencia y trabajar directamente con la base de datos de SQL Server. El esquema de pertenencia incluye un procedimiento almacenado denominado `aspnet_Membership_SetPassword` que establece una contraseña de usuario y no requiere la respuesta de seguridad o la contraseña antigua para realizar su tarea.

Ninguna de estas opciones son especialmente atractiva, pero es cómo va a veces la vida de un desarrollador.

Fui con antelación e implementa el tercer enfoque, escribir código que omite el `Membership` y `MembershipUser` clases y opera directamente el `SecurityTutorials` base de datos.

> [!NOTE]
> Cuando se trabaja directamente con la base de datos, se daña la encapsulación proporcionada por el marco de pertenencia. Esta decisión nos vincula el `SqlMembershipProvider`, por lo que nuestro código menos portable. Además, este código puede no funcionar según lo previsto en futuras versiones de ASP.NET si cambia el esquema de pertenencia. Este enfoque es una solución alternativa y, al igual que la mayoría de las soluciones alternativas, no es un ejemplo de procedimientos recomendados.


El código tiene algunos bits poco atractivo y es bastante largo. Por lo tanto, no quiero provocar un desorden en este tutorial con un estudio detallado del mismo. Si está interesado en obtener más información, descargue el código para este tutorial y visite el `~/Administration/ManageUsers.aspx` página. Esta página, que hemos creado en el <a id="_msoanchor_5"> </a> [tutorial anterior](building-an-interface-to-select-one-user-account-from-many-cs.md), enumera cada usuario. He actualizado el control GridView para incluir un vínculo a la `UserInformation.aspx` página, pasando el nombre de usuario del usuario seleccionado a través de la cadena de consulta. El `UserInformation.aspx` página muestra información sobre el usuario seleccionado y los cuadros de texto para cambiar su contraseña (consulte la figura 9).

Después de escribir la nueva contraseña, que se confirma en el segundo cuadro de texto y, al hacer clic en el botón de actualización de usuario, una devolución de datos que habrá trastornos y `aspnet_Membership_SetPassword` se invoca el procedimiento almacenado, actualizar la contraseña del usuario. Lo animo a aquellos lectores interesados en esta funcionalidad esté más familiarizados con el código y vuelva a extender la funcionalidad para incluir enviar un correo electrónico al usuario cuya contraseña se cambió.


[![Un administrador puede cambiar la contraseña de un usuario](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Figura 9**: un administrador puede cambiar la contraseña de un usuario ([haga clic aquí para ver imagen en tamaño completo](recovering-and-changing-passwords-cs/_static/image27.png))


> [!NOTE]
> El `UserInformation.aspx` página actualmente solo funciona si el marco de pertenencia está configurado para almacenar contraseñas en formato cifrado o Hashed. Contiene el código para cifrar la contraseña nueva, aunque lo invitamos a agregar esta funcionalidad. Es la manera en que recomienda agregar el código necesario con un descompilador como [Reflector](http://www.aisto.com/roeder/dotnet/) para examinar el código fuente para los métodos de .NET Framework; comenzar examinando la `SqlMembershipProvider` la clase `ChangePassword` método. Ésta es la técnica que usa para escribir el código para crear un hash de la contraseña.


## <a name="summary"></a>Resumen

ASP.NET ofrece dos controles para ayudar a los usuarios a administrar su contraseña. El control PasswordRecovery es útil para aquellos que han olvidado su contraseña. Según la configuración del marco de pertenencia, el usuario se envía o su contraseña existente o una nueva contraseña generada de forma aleatoria. El control ChangePassword permite que un usuario actualice su contraseña.

Al igual que los controles de inicio de sesión y CreateUserWizard, los controles PasswordRecovery y ChangePassword representan una interfaz de usuario enriquecida sin tener que escribir ni una pizca de marcado declarativo o línea de código. Si la interfaz de usuario predeterminada no satisface sus necesidades, puede personalizarlo a través de una serie de propiedades de estilo. Como alternativa, las interfaces de los controles se pueden convertir en plantillas, para un grado de control más fino si cabe. En segundo plano estos controles usa la API de pertenencia, invocar el `MembershipUser` del objeto `ResetPassword` y `ChangePassword` métodos.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Control ChangePassword guías de inicio rápido](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Control PasswordRecovery guías de inicio rápido](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Envío de correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` Preguntas más frecuentes](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial incluyen Michael Emmings y Suchi Banerjee. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](building-an-interface-to-select-one-user-account-from-many-cs.md)
> [Siguiente](unlocking-and-approving-user-accounts-cs.md)
