---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: "Recuperar y cambiar las contraseñas (VB) | Documentos de Microsoft"
author: rick-anderson
description: "ASP.NET incluye dos controles Web de ayudar a recuperar y cambiar las contraseñas. El control PasswordRecovery permite un visitante que se va a recuperar a su pa pierde..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: b78469858483a9501a0f73d1c894e29ae0a99122
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="recovering-and-changing-passwords-vb"></a>Recuperar y cambiar las contraseñas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) o [descarga de PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET incluye dos controles Web de ayudar a recuperar y cambiar las contraseñas. El control PasswordRecovery permite un visitante que se va a recuperar su contraseña perdida. El control ChangePassword permite al usuario que actualice su contraseña. Al igual que otros controles relacionados con el inicio de sesión Web que vimos a lo largo de esta serie de tutoriales, la PasswordRecovery y ChangePassword controla el trabajo con el marco de trabajo de pertenencia en segundo plano para restablecer o modificar las contraseñas de usuario.


## <a name="introduction"></a>Introducción

Entre los sitios Web para mi bank, empresa de utilidad, compañía telefónica, cuentas de correo electrónico y los portales web personalizado, si, al igual que la mayoría de los usuarios, dispone de docenas de tener que recordar contraseñas diferentes. Con tantas credenciales para memorizar hoy en día, no es raro olvidado su contraseña. Para tener esto en cuenta, sitios Web que ofrecen las cuentas de usuario debe incluir una manera para que un usuario recuperar su contraseña. Este proceso conlleva normalmente generar una nueva contraseña aleatoria y enviarla a la dirección de correo electrónico del usuario en el archivo. Después de recibir su nueva contraseña mayoría de los usuarios volver al sitio y cambia la contraseña de la generado aleatoriamente a uno más fácil de recordar.

ASP.NET incluye dos controles Web de ayudar a recuperar y cambiar las contraseñas. El control PasswordRecovery permite un visitante que se va a recuperar su contraseña perdida. El control ChangePassword permite al usuario que actualice su contraseña. Al igual que otros controles relacionados con el inicio de sesión Web que vimos a lo largo de esta serie de tutoriales, la PasswordRecovery y ChangePassword controla el trabajo con el marco de trabajo de pertenencia en segundo plano para restablecer o modificar las contraseñas de usuario.

En este tutorial examinaremos mediante estos dos controles. También veremos cómo cambiar mediante programación y restablecer la contraseña de un usuario a través de la `MembershipUser` la clase `ChangePassword` y `ResetPassword` métodos.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Paso 1: Ayudar a los usuarios recuperar perdido contraseñas

Todos los sitios Web que admiten las cuentas de usuario deben proporcionar a los usuarios con algún mecanismo para recuperar sus contraseñas olvidadas. Lo bueno es que implementar esta funcionalidad en ASP.NET es muy sencilla gracias a control PasswordRecovery Web. El control PasswordRecovery representa una interfaz que pide al usuario su nombre de usuario y, si es necesario, la respuesta a su pregunta de seguridad. Lo envíe por correo electrónico al usuario su contraseña.

> [!NOTE]
> Dado que los mensajes de correo electrónico se transmiten a través de la conexión en texto sin formato intervienen los riesgos de seguridad con el envío de contraseña de un usuario por correo electrónico.


El control PasswordRecovery consta de tres vistas:

- **Nombre de usuario** -pide al visitante su nombre de usuario. Se trata de la vista inicial.
- **Pregunta**-pregunta de seguridad y el nombre de usuario del usuario se muestra como texto, junto con un cuadro de texto para el usuario que escriba la respuesta a su pregunta de seguridad.
- **Éxito**-muestra un mensaje que informa al usuario que se ha enviado su contraseña.

Muestran las vistas y las acciones realizadas por el control PasswordRecovery dependen de las siguientes opciones de configuración de pertenencia:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

El marco de trabajo de pertenencia `RequiresQuestionAndAnswer` valor indica si los usuarios deben especificar una pregunta de seguridad y la respuesta al registrarse para una cuenta. Como se explicó en la <a id="_msoanchor_1"> </a> [ *crear cuentas de usuario* ](../membership/creating-user-accounts-vb.md) tutorial, if `RequiresQuestionAndAnswer` es True (valor predeterminado), a continuación, la interfaz del control CreateUserWizard incluye el cuadro de texto controles para la pregunta de seguridad del nuevo usuario y la respuesta; Si `RequiresQuestionAndAnswer` es False, se recopila ninguna información de este tipo. De forma similar, si `RequiresQuestionAndAnswer` es True, el control PasswordRecovery muestra la pregunta ver después de que el usuario ha escrito su nombre de usuario, ya que la contraseña es recuperar sólo si el usuario escribe la respuesta de seguridad correcta. Si `RequiresQuestionAndAnswer` es False, sin embargo, el control PasswordRecovery mueve directamente desde la vista de nombre de usuario a la vista correcto.

Después de que el usuario ha proporcionado su nombre de usuario - o su respuesta de nombre de usuario y seguridad, si `RequiresQuestionAndAnswer` es True: el PasswordRecovery envía por correo electrónico al usuario la contraseña. Si el `EnablePasswordRetrieval` opción está establecida en True, a continuación, el usuario se envían por correo electrónico su contraseña actual. Si se establece en False y `EnablePasswordReset` está establecida en True, el control PasswordRecovery genera una nueva contraseña aleatoria para el usuario y se envía por correo electrónico esta nueva contraseña a ellos. Si ambos `EnablePasswordRetrieval` y `EnablePasswordReset` son False, el control PasswordRecovery produce una excepción.

> [!NOTE]
> Recuerde que el `SqlMembershipProvider` almacena las contraseñas de usuario en uno de estos tres formatos: Clear, Hashed (valor predeterminado) o cifrado. El mecanismo de almacenamiento utilizado depende de los valores de configuración de pertenencia; la aplicación de demostración utiliza el formato de la contraseña Hashed. Cuando se usa el formato de la contraseña Hashed el `EnablePasswordRetrieval` opción debe establecerse en False porque el sistema no puede determinar la contraseña del usuario real de la versión de hash almacenado en la base de datos.


La figura 1 muestra cómo el PasswordRecovery interfaz y el comportamiento se ve afectado por la configuración de pertenencia.


[![Los RequiresQuestionAndAnswer, EnablePasswordRetrieval y EnablePasswordReset influir en apariencia y el comportamiento del control PasswordRecovery](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Figura 1**: el `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, y `EnablePasswordReset` influir en la apariencia y el comportamiento del control PasswordRecovery ([haga clic aquí para ver la imagen a tamaño completo](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> En el <a id="_msoanchor_2"> </a> [ *crear el esquema de pertenencia en SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) tutorial hemos configurado el proveedor de pertenencia estableciendo `RequiresQuestionAndAnswer` en True, `EnablePasswordRetrieval` a False, y `EnablePasswordReset` en True.


### <a name="using-the-passwordrecovery-control"></a>Uso del Control PasswordRecovery

Echemos un vistazo a usar el control PasswordRecovery en una página ASP.NET. Abra `RecoverPassword.aspx` y arrastre y coloque un control PasswordRecovery desde el cuadro de herramientas hasta el diseñador; establezca su `ID` a `RecoverPwd`. Al igual que los controles de inicio de sesión y CreateUserWizard Web, vistas del control PasswordRecovery representan una interfaz enriquecida compuesta que incluye las etiquetas, cuadros de texto, botones y controles de validación. Puede personalizar la apariencia de las vistas mediante las propiedades de estilo del control o mediante la conversión de las vistas en plantillas. Dejar este como un ejercicio para el lector interesado.

Cuando un usuario visita esta página se escriba su nombre de usuario y haga clic en el botón Enviar. Dado que hemos configurado la `RequiresQuestionAndAnswer` propiedad como True en nuestra configuración de pertenencia, el PasswordRecovery controlar will, a continuación, mostrar la vista de pregunta. Cuando el usuario escribe su respuesta de seguridad correcta y haga clic en enviar, el control PasswordRecovery actualizará la contraseña del usuario a una generada de forma aleatoria y esta contraseña a la dirección de correo electrónico en el archivo de correo electrónico. Todo esto era posible sin que tengamos que escribir una sola línea de código.

Antes de probar esta página, hay una parte final de la configuración para tienden a: es preciso especificar la configuración de entrega de correo electrónico en `Web.config`. El control PasswordRecovery se basa en esta configuración para enviar el correo electrónico.

Se especifica la configuración de entrega de correo electrónico a través de la [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx)del [ `<mailSettings>` elemento](https://msdn.microsoft.com/library/w355a94k.aspx). Use la [ `<smtp>` elemento](https://msdn.microsoft.com/library/ms164240.aspx) para indicar el método de entrega y el valor predeterminado de la dirección. El siguiente marcado configura las opciones de correo electrónico para usar un servidor SMTP de red denominado `smtp.example.com` en el puerto 25 y con las credenciales de usuario y la contraseña de usuario y la contraseña.

> [!NOTE]
> `<system.net>`es un elemento secundario de la raíz de `<configuration>` elemento y un elemento relacionado de `<system.web>`. Por lo tanto, no ponga el `<system.net>` elemento dentro de la `<system.web>` elemento; en su lugar, se coloca en el mismo nivel.


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

Además de utilizar un servidor SMTP en la red, o bien puede especificar un directorio de recogida donde se deben depositar para enviar mensajes de correo electrónico.

Una vez haya configurado la configuración de SMTP, visite la `RecoverPassword.aspx` página a través de un explorador. En primer lugar intenta escribir un nombre de usuario que no existe en el almacén del usuario. Como se muestra en la figura 2, el control PasswordRecovery muestra un mensaje que indica que no tener acceso a la información de usuario. Se puede personalizar el texto del mensaje a través del control [ `UserNameFailureText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Se muestra un mensaje de Error si se especifica un nombre de usuario no válido](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Figura 2**: aparece un mensaje de Error si se especifica un nombre de usuario no válido ([haga clic aquí para ver la imagen a tamaño completo](recovering-and-changing-passwords-vb/_static/image6.png))


Ahora, escriba un nombre de usuario. Use el nombre de usuario de una cuenta en el sistema con una dirección de correo electrónico que puede tener acceso y responder a cuya seguridad se conoce. Después de escribir el nombre de usuario y hacer clic en enviar, el control PasswordRecovery muestra la vista de pregunta. Como con la vista de nombre de usuario, si escribe incorrecta responder el control PasswordRecovery muestra un mensaje de error (consulte la figura 3). Use la [ `QuestionFailureText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) para personalizar este mensaje de error.


[![Se muestra un mensaje de Error si el usuario escribe una respuesta de seguridad no válido](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Figura 3**: aparece un mensaje de Error si el usuario escribe una respuesta de seguridad no válido ([haga clic aquí para ver la imagen a tamaño completo](recovering-and-changing-passwords-vb/_static/image9.png))


Por último, escriba la respuesta de seguridad correcta y haga clic en enviar. En segundo plano, el control PasswordRecovery genera una contraseña aleatoria, lo asigna a la cuenta de usuario, envía un correo electrónico que informa al usuario de su nueva contraseña (consulte la figura 4) y, a continuación, muestra la vista correcto.


[![El usuario se envía un correo electrónico con His nueva contraseña](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Figura 4**: el usuario se envía un correo electrónico con la nueva contraseña His ([haga clic aquí para ver la imagen a tamaño completo](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>Personalizar el correo electrónico

El correo electrónico predeterminado enviado por el control PasswordRecovery es bastante aburrido (consulte la figura 4). El mensaje se envía desde la cuenta especificada en el `<smtp>` del elemento `from` atributo con la contraseña de asunto y el cuerpo de texto sin formato:

Por favor, vuelva al sitio e inicie sesión con la siguiente información.

Nombre de usuario: *nombre de usuario*

contraseña: *contraseña*

Este mensaje se puede personalizar mediante programación a través de un controlador de eventos para el control PasswordRecovery [ `SendingMail` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), o mediante declaración a través del [ `MailDefinition` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Vamos a examinar estas dos opciones.

El `SendingMail` se desencadena el evento justo antes de que el mensaje de correo electrónico se envía y es la última oportunidad para ajustar mediante programación el mensaje de correo electrónico. Cuando se genera este evento, el controlador de eventos se pasa un objeto de tipo [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), cuyo `Message` propiedad contiene una referencia al correo electrónico que vaya a enviarse.

Crear un controlador de eventos para el `SendingMail` eventos y agregue el código siguiente, que agrega mediante programación `webmaster@example.com` a la lista CC.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

El mensaje de correo electrónico también pueden configurarse de manera declarativa. El PasswordRecovery `MailDefinition` propiedad es un objeto de tipo [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). El `MailDefinition` clase ofrece una gran cantidad de propiedades relacionadas con el correo electrónico, incluyendo `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`y otros. Para empezar, establecer el [ `Subject` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) a algo más descriptivo que el se utiliza de forma predeterminada (contraseña), como se ha restablecido la contraseña...

Para personalizar el cuerpo del mensaje de correo electrónico que es necesario para crear un archivo de plantilla de correo electrónico independiente que contiene el contenido del cuerpo. Empiece por crear una nueva carpeta en el sitio Web denominado `EmailTemplates`. A continuación, agregue un nuevo archivo de texto a esta carpeta con el nombre `PasswordRecovery.txt` y agregue el siguiente contenido:

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Tenga en cuenta el uso de los marcadores de posición `<%UserName%>` y `<%Password%>`. El control PasswordRecovery reemplaza automáticamente estos dos marcadores de posición con nombre de usuario y una contraseña recuperada antes de enviar el correo electrónico del usuario.

Por último, elija la `MailDefinition`del [ `BodyFileName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) a la plantilla de correo electrónico que acabamos de crear (`~/EmailTemplates/PasswordRecovery.txt`).

Después de realizar estos cambios en volver a revisar la `RecoverPassword.aspx` página y escriba su nombre de usuario y seguridad de la respuesta. Recibirá debe un correo electrónico que parece similar al que se muestra en la figura 5. Tenga en cuenta que `webmaster@example.com` ha sido CC cabe y que se han actualizado el asunto y cuerpo.


[![Se ha actualizado el asunto, el cuerpo y la lista CC](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Figura 5**: el asunto, cuerpo y CC se han actualizado la lista ([haga clic aquí para ver la imagen a tamaño completo](recovering-and-changing-passwords-vb/_static/image15.png))


Para enviar un correo electrónico con formato HTML establece [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) en True (el valor predeterminado es False) y actualización de la plantilla de correo electrónico para incluir HTML.

El `MailDefinition` propiedad no es única para la clase PasswordRecovery. Como se verá en el paso 2, el control ChangePassword también ofrece un `MailDefinition` propiedad. Además, el control CreateUserWizard incluye este tipo de propiedad que se pueden configurar para enviar automáticamente un mensaje de correo electrónico de bienvenida a los nuevos usuarios.

> [!NOTE]
> Actualmente no hay ningún vínculo en el panel de navegación izquierdo para llegar a la `RecoverPassword.aspx` página. Un usuario solo se esté interesado en visitar esta página si no puede iniciar sesión correctamente en el sitio. Por lo tanto, actualizar la `Login.aspx` página para incluir un vínculo a la `RecoverPassword.aspx` página.


### <a name="programmatically-resetting-a-users-password"></a>Mediante programación al restablecer una contraseña de usuario

Cuando el restablecimiento de contraseña de un usuario la PasswordRecovery control llama el `MembershipUser` del objeto [ `ResetPassword` método](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Este método tiene dos sobrecargas:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)**-restablece una contraseña de usuario. Utilice esta sobrecarga si `RequiresQuestionAndAnswer` es False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)**-Restablece solo si de contraseña de un usuario proporcionado *securityAnswer* es correcta. Utilice esta sobrecarga si `RequiresQuestionAndAnswer` es True.

Ambas sobrecargas devuelven la nueva contraseña generada de forma aleatoria.

Al igual que con los otros métodos en el marco de trabajo de pertenencia, el `ResetPassword` método delega en el proveedor configurado. El `SqlMembershipProvider` , se invoca el `aspnet_Membership_ResetPassword` procedimiento almacenado, pasando el nombre de usuario, la nueva contraseña y la respuesta de contraseña proporcionada, entre otros campos. El procedimiento almacenado se asegura de que la respuesta de contraseña coincide con y, a continuación, actualiza la contraseña del usuario.

Un par de notas de la implementación de nivel inferior:

- Un usuario bloqueado no puede restablecer su contraseña. Sin embargo, puede un usuario no autorizado. Se tratará la bloqueado fuera y aprueba estados con más detalle en la <a id="_msoanchor_3"> </a> [ *Unlocking y aprobar usuario* ](unlocking-and-approving-user-accounts-vb.md) tutorial de cuentas.
- Si la respuesta de contraseña es incorrecta, se incrementa el recuento de intentos de respuesta de contraseña del usuario. Si se produce un número especificado de intentos de respuesta de seguridad no válido dentro de un período de tiempo especificado, el usuario está bloqueado.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Se generan una palabra en cómo el contraseñas aleatorias

Las contraseñas generada de forma aleatoria que se muestra en los mensajes de correo electrónico en las figuras 4 y 5 se crean mediante la clase de pertenencia [ `GeneratePassword` método](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Este método acepta dos parámetros de entrada enteros - *longitud* y *numberOfNonAlphanumericCharacters* - y devuelve una cadena de al menos *longitud* long con los caracteres en menos *numberOfNonAlphanumericCharacters* número de caracteres no alfanuméricos. Cuando este método se llama desde dentro de las clases de pertenencia o los controles relacionados con el inicio de sesión Web, los valores para estos dos parámetros se determinan mediante la configuración de pertenencia `MinRequiredPasswordLength` y `MinRequiredNonalphanumericCharacters` propiedades, que se establecen en 7 y 1, respectivamente.

El `GeneratePassword` método usa un generador de números aleatorios criptográficamente seguro para asegurarse de que no hay ninguna diferencia en los caracteres aleatorios se seleccionan. Además, `GeneratePassword` es `Public`, lo que significa que se puede usarlo directamente desde la aplicación de ASP.NET que necesita para generar cadenas aleatorias o contraseñas.

> [!NOTE]
> El `SqlMembershipProvider` clase siempre genera una contraseña aleatoria al menos de 14 caracteres, por lo que si `MinRequiredPasswordLength` es inferior a 14, a continuación, se omite su valor.


## <a name="step-2-changing-passwords"></a>Paso 2: Cambiar las contraseñas

Las contraseñas generada de forma aleatoria son difíciles de recordar. Tenga en cuenta la contraseña que se muestra en la figura 4: `WWGUZv(f2yM:Bd`. Intente confirmar en la memoria. Sobra decir cuando un usuario se envía una contraseña generada de forma aleatoria de este tipo, pero podrá desea cambiar la contraseña a algo más fácil de recordar.

Utilice el control ChangePassword para crear una interfaz para que un usuario cambie su contraseña. Mucho al igual que el control PasswordRecovery, el control ChangePassword consta de dos vistas: cambiar contraseña y correcto. La vista Cambiar contraseña solicita al usuario sus contraseñas antiguas y nuevas. Tras proporcionar la contraseña antigua correcta y una nueva contraseña que cumple los requisitos de carácter no alfanumérico y una longitud mínima, el control ChangePassword actualiza la contraseña del usuario y muestra la vista correcto.

> [!NOTE]
> El control ChangePassword modifica la contraseña del usuario mediante la invocación de la `MembershipUser` del objeto [ `ChangePassword` método](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). El método ChangePassword acepta dos `String` entrada parámetros - *oldPassword* y *newPassword*- y actualiza la cuenta de usuario con el *newPassword*, Suponiendo que el proporcionado *oldPassword* es correcta.


Abra la `ChangePassword.aspx` página y agregar un control ChangePassword a la página, asígnele el nombre `ChangePwd`. En este momento, la vista de diseño debe mostrar cambiar contraseña ver (consulte la figura 6). Al igual que con el control PasswordRecovery, puede alternar entre las vistas a través de la etiqueta inteligente del control. Además, las repeticiones de estas vistas son personalizables a través de las propiedades de estilo ordenados o convertirlos a una plantilla.


[![Agregar un Control ChangePassword a la página](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Figura 6**: agregar un ChangePassword Control a la página ([haga clic aquí para ver la imagen a tamaño completo](recovering-and-changing-passwords-vb/_static/image18.png))


El control ChangePassword puede actualizar la contraseña del usuario conectado actualmente *o* la contraseña de otro usuario especificado. Como se muestra en la figura 6, la vista de cambiar la contraseña predeterminada representa solo tres controles de cuadro de texto: uno para la contraseña antigua y dos para la nueva contraseña. Esta interfaz predeterminada se utiliza para actualizar la contraseña del usuario ha iniciado sesión actualmente.

Para usar el control ChangePassword para actualizar la contraseña de otro usuario, establezca el control [ `DisplayUserName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) en True. Esta forma, agregará a la página, pedir el nombre de usuario del usuario cuya contraseña para cambiar un cuarto cuadro de texto.

Establecer `DisplayUserName` a True es útil si desea permitir que un usuario que ha iniciado a cambiar su contraseña sin tener que iniciar sesión. Personalmente, creo que no hay ningún problema con obligar al usuario al inicio de sesión antes de permitir que ella cambiar su contraseña. Por lo tanto, deje `DisplayUserName` establecida en False (su valor predeterminado). Para tomar esta decisión, sin embargo, es básicamente estamos salvo los usuarios anónimos de llegar a esta página. Actualizar las reglas de autorización de dirección URL del sitio con el fin de impedir que los usuarios anónimos visita `ChangePassword.aspx`. Si tiene que actualizar la memoria en la sintaxis de regla de autorización de URL, hacen referencia a la <a id="_msoanchor_4"> </a> [ *autorización basada en usuario* ](../membership/user-based-authorization-vb.md) tutorial.

> [!NOTE]
> Puede parecer que el `DisplayUserName` propiedad es útil para permitir que los administradores cambiar las contraseñas de otros usuarios. Sin embargo, incluso cuando `DisplayUserName` está establecido en True debe conocerse la contraseña antigua correcta y especificada. Hablaremos sobre las técnicas para permitir que los administradores cambiar las contraseñas de usuario en el paso 3.


Visite la `ChangePassword.aspx` página a través de un explorador y cambiar la contraseña. Tenga en cuenta que se muestra un mensaje de error si escribe una nueva contraseña que no cumple los requisitos de caracteres no alfanuméricos especificados en la configuración de pertenencia y de longitud de la contraseña (consulte la figura 7).


[![Agregar un Control ChangePassword a la página](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Figura 7**: agregar un ChangePassword Control a la página ([haga clic aquí para ver la imagen a tamaño completo](recovering-and-changing-passwords-vb/_static/image21.png))


Cuando se especifica la contraseña antigua correcta y una contraseña nueva válida, el inicio de sesión usuario de la contraseña se cambia y muestra la vista correcto.

### <a name="sending-a-confirmation-email"></a>Enviar un correo electrónico de confirmación

De forma predeterminada, el control ChangePassword no envía un mensaje de correo electrónico al usuario cuya contraseña se haya actualizado. Si desea enviar un correo electrónico, simplemente configurar el control `MailDefinition` propiedad. Vamos a configurar el control ChangePassword para que el usuario se envíe un correo electrónico con formato HTML que contiene su nueva contraseña.

Empiece por crear un archivo nuevo en el `EmailTemplates` carpeta denominada `ChangePassword.htm`. Agregue el siguiente marcado:

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

A continuación, establezca el control ChangePassword `MailDefinition` la propiedad `BodyFileName`, `IsBodyHtml`, y `Subject` propiedades a ~ / EmailTemplates/ChangePassword.htm, True y la contraseña ha cambiado!, respectivamente.

Después de realizar estos cambios, volver a visitar la página y cambiar la contraseña de nuevo. En esta ocasión, el control ChangePassword envía un mensaje personalizado, con formato HTML a dirección de correo electrónico del usuario en el archivo (consulte la figura 8).


[![Un mensaje de correo electrónico le informa de que ha cambiado la contraseña del usuario que sus](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Figura 8**: un mensaje de correo electrónico indica que ha cambiado la contraseña del usuario que Their ([haga clic aquí para ver la imagen a tamaño completo](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Paso 3: Permitir que los administradores cambiar las contraseñas de usuario

Una característica común en las aplicaciones que admiten las cuentas de usuario es la capacidad para un usuario administrativo cambiar las contraseñas de otros usuarios. A veces, esta funcionalidad es necesaria porque el sistema no tiene la posibilidad de que los usuarios que cambien sus contraseñas. En tal caso, la única forma de un usuario recuperar su contraseña olvidada sería el administrador les asigne una nueva contraseña. Con los controles PasswordRecovery y ChangePassword, sin embargo, los usuarios administrativos necesitan no ocupado por sí mismos con el cambio de contraseña de los usuarios, como los usuarios son capaces de hacer este por sí mismos.

Pero ¿qué ocurre si el cliente insiste en que los usuarios administrativos deberían poder cambiar las contraseñas de otros usuarios? Por desgracia, agregar esta funcionalidad puede ser un poco de trabajo. Para cambiar la contraseña de un usuario, se debe proporcionar la contraseña antigua y nueva a la `MembershipUser` del objeto `ChangePassword` método, pero un administrador no debería tener que conocer la contraseña de un usuario para modificarlo.

Una solución es primero restablecer la contraseña del usuario y, a continuación, cámbielo a la nueva contraseña mediante código similar al siguiente:

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Este código se inicia mediante la recuperación de información sobre *nombre de usuario*, que es el usuario cuya contraseña desea cambiar el administrador. Después, el `ResetPassword` se invoca el método, que asigna y el usuario una nueva contraseña aleatoria. Esta contraseña generada de forma aleatoria es devuelto por el método y se almacena en la variable `resetPwd`. Ahora que sabemos que la contraseña del usuario, se puede cambiar mediante una llamada a `ChangePassword`.

El problema es que este código sólo funciona si se establece la configuración del sistema de pertenencia que `RequiresQuestionAndAnswer` es False. Si `RequiresQuestionAndAnswer` es True, ya que es con nuestra aplicación, el `ResetPassword` método deba pasarse a la respuesta de seguridad, en caso contrario, producirá una excepción.

Si el marco de trabajo de pertenencia está configurado para solicitar una pregunta de seguridad y la respuesta, y aún su cliente insiste en que los administradores podrán cambiar las contraseñas de usuario, tiene tres opciones:

- Iniciar las manos en el ambiente y saber que el cliente que se trata de un solo lo que no se puede realizar.
- Establecer `RequiresQuestionAndAnswer` en False. Esto da como resultado una aplicación menos segura. Imagine que un usuario malintencionado ha obtenido acceso a la Bandeja de entrada de correo electrónico de otro usuario. Quizás el usuario en peligro ha abandonado su puesto de trabajo para ir a comer y no bloquea su estación de trabajo, o quizás que tiene acceso a su correo electrónico desde un terminal público y no cierre la sesión. En cualquier caso, el usuario malintencionado puede visitar el `RecoverPassword.aspx` página y escriba el nombre de usuario. El sistema, a continuación, enviará la contraseña recuperada sin pedir la respuesta de seguridad.
- Omitir la capa de abstracción de creado por el marco de pertenencia y trabajar directamente con la base de datos de SQL Server. El esquema de la pertenencia incluye un procedimiento almacenado denominado `aspnet_Membership_SetPassword` que establece la contraseña de un usuario y no requiere la contraseña antigua o respuesta de seguridad con el fin de realizar su tarea.

Ninguna de estas opciones son especialmente atractiva, pero es cómo va a veces la vida de un desarrollador.

Produjo por anticipado e implementa el tercer enfoque, escribir código que omite el `Membership` y `MembershipUser` clases y funciona directamente en el `SecurityTutorials` base de datos.

> [!NOTE]
> Cuando se trabaja directamente con la base de datos, se daña la encapsulación que proporciona el marco de trabajo de pertenencia. Esta decisión que vincula la `SqlMembershipProvider`, nuestro código es menos portátil. Además, este código no funcionen según lo previsto en futuras versiones de ASP.NET si cambia el esquema de pertenencia. Este enfoque es una solución y, al igual que la mayoría de las soluciones alternativas, no es un ejemplo de procedimientos recomendados.


El código tiene algunos bits poco atractivo y es muy largo. Por lo tanto, no deseo desordenan este tutorial con un estudio detallado del mismo. Si está interesado en aprender más, descargar el código de este tutorial y, visite la `~/Administration/ManageUsers.aspx` página. Esta página, que hemos creado en el <a id="_msoanchor_5"> </a> [tutorial anterior](building-an-interface-to-select-one-user-account-from-many-vb.md), enumera cada usuario. He actualicé GridView para incluir un vínculo a la `UserInformation.aspx` página, pasando el nombre de usuario del usuario seleccionado a través de la cadena de consulta. La `UserInformation.aspx` página muestra información sobre el usuario seleccionado y los cuadros de texto para cambiar su contraseña (consulte la figura 9).

Después de escribir la nueva contraseña, confirmar en el segundo cuadro de texto y haga clic en el botón de usuario de actualización, tiene lugar una devolución de datos y la `aspnet_Membership_SetPassword` se invoca el procedimiento almacenado, actualizar la contraseña del usuario. Instar a los lectores interesados en esta funcionalidad para familiarizarse con el código e intentar extender la funcionalidad para incluir el envío de correo electrónico al usuario cuya contraseña se cambió.


[![Un administrador puede cambiar la contraseña de un usuario](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Figura 9**: un administrador puede cambiar la contraseña de un usuario ([haga clic aquí para ver la imagen a tamaño completo](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> La `UserInformation.aspx` página actualmente sólo funciona si el marco de trabajo de pertenencia está configurado para almacenar contraseñas en formato cifrado o Hashed. Contiene el código para cifrar la contraseña nueva, aunque se le invita a agregar esta funcionalidad. Es la manera en que recomienda agregar el código necesario utilizar un descompilador como [Reflector](http://www.aisto.com/roeder/dotnet/) para examinar el código fuente para los métodos de .NET Framework; iniciar examinando el `SqlMembershipProvider` la clase `ChangePassword` método. Ésta es la técnica que usa para escribir el código para crear un hash de la contraseña.


## <a name="summary"></a>Resumen

ASP.NET ofrece dos controles para ayudar a los usuarios a administrar su contraseña. El control PasswordRecovery es útil para aquellos que han olvidado su contraseña. Dependiendo de la configuración de la pertenencia del marco de trabajo, el usuario o se envía su contraseña existente o una nueva contraseña generada de forma aleatoria. El control ChangePassword permite que un usuario actualice su contraseña.

Al igual que los controles de inicio de sesión y CreateUserWizard, los controles PasswordRecovery y ChangePassword representan una interfaz de usuario enriquecida sin tener que escribir ni una sola línea de marcado declarativo o línea de código. Si la interfaz de usuario predeterminada no satisface sus necesidades, puede personalizarlo a través de una serie de propiedades de estilo. Como alternativa, interfaces de los controles se pueden convertir en plantillas, para un incluso mayor grado de control. Segundo plano estos controles utilizan la API de pertenencia, invocar el `MembershipUser` del objeto `ResetPassword` y `ChangePassword` métodos.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Tutoriales del Control ChangePassword](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Tutoriales del Control PasswordRecovery](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Enviar correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail`Preguntas más frecuentes](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial incluyen Michael Emmings y Suchi Banerjee. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](building-an-interface-to-select-one-user-account-from-many-vb.md)
[Siguiente](unlocking-and-approving-user-accounts-vb.md)
