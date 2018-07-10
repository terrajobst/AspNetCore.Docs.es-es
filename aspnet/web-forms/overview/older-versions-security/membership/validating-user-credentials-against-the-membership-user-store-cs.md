---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Validar las credenciales de usuario en el Store del usuario de pertenencia (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, examinaremos cómo validar las credenciales del usuario en el almacén de usuario de pertenencia mediante medios de programación y el control de inicio de sesión...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: e8c46d09a7ebab19204f7c439ec4333e0c36b73e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828960"
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>Validar las credenciales de usuario en el Store del usuario de pertenencia (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) o [descargar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> En este tutorial, examinaremos cómo validar las credenciales del usuario en el almacén de usuario de pertenencia mediante medios de programación y el control de inicio de sesión. También veremos cómo personalizar la apariencia y el comportamiento del control de inicio de sesión.


## <a name="introduction"></a>Introducción

En el <a id="Tutorial05"> </a> [tutorial anterior](creating-user-accounts-cs.md) analizamos cómo crear una nueva cuenta de usuario en el marco de pertenencia. Primero analizamos crear mediante programación cuentas de usuario mediante el `Membership` la clase `CreateUser` método y, a continuación, examinar utilizando el control CreateUserWizard Web. Sin embargo, la página de inicio de sesión actualmente valida las credenciales proporcionadas con una lista codificada de pares de nombre de usuario y contraseña. Es necesario actualizar la lógica de la página de inicio de sesión para que lo valida las credenciales en el almacén de usuario del marco de pertenencia.

Mucho al igual que con la creación de cuentas de usuario, las credenciales se pueden validar mediante programación o declaración. La API de pertenencia incluye un método para validar mediante programación las credenciales del usuario en el almacén de usuarios. Y ASP.NET se suministra con el control Web de inicio de sesión, que representa una interfaz de usuario con los cuadros de texto para el nombre de usuario y contraseña y un botón para iniciar sesión.

En este tutorial, examinaremos cómo validar las credenciales del usuario en el almacén de usuario de pertenencia mediante medios de programación y el control de inicio de sesión. También veremos cómo personalizar la apariencia y el comportamiento del control de inicio de sesión. Comencemos.

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Paso 1: Validar las credenciales en el Store del usuario de pertenencia

Para los sitios web que usan la autenticación de formularios, un usuario inicia sesión en el sitio Web al visitar una página de inicio de sesión y especificar sus credenciales. Estas credenciales, a continuación, se comparan con el almacén del usuario. Si son válidos, se concede al usuario un vale de autenticación de formularios, que es un token de seguridad que indica la identidad y la autenticidad del visitante.

Para validar a los usuarios con el marco de pertenencia, utilice el `Membership` la clase [ `ValidateUser` método](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). El `ValidateUser` método toma dos parámetros de entrada - *`username`* y *`password`* - y devuelve un valor booleano que indica si las credenciales son válidas. Al igual que con el `CreateUser` método hemos visto en el tutorial anterior, el `ValidateUser` método delega la validación real para el proveedor de pertenencia configurado.

El `SqlMembershipProvider` valida las credenciales proporcionadas mediante la obtención de la contraseña del usuario especificado a través de la `aspnet_Membership_GetPasswordWithFormat` procedimiento almacenado. Recuerde que el `SqlMembershipProvider` almacena las contraseñas de usuario mediante uno de los tres formatos: borrar, cifrados o aplicar un algoritmo hash. El `aspnet_Membership_GetPasswordWithFormat` procedimiento almacenado devuelve la contraseña en su formato sin procesar. Para las contraseñas cifradas o algoritmo hash, el `SqlMembershipProvider` transforma el *`password`* valor se pasa a la `ValidateUser` método en su equivalente cifrados o aplicar un algoritmo hash de estado y, a continuación, se compara con lo que se devolvió desde el base de datos. Si la contraseña almacenada en la base de datos coincide con la contraseña con formato especificada por el usuario, las credenciales son válidas.

Vamos a actualizar nuestra página de inicio de sesión (~ /`Login.aspx`) para que lo valida las credenciales proporcionadas en el almacén de usuario del marco de pertenencia. Hemos creado esta página de inicio de sesión en el <a id="Tutorial02"> </a> [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, crear una interfaz con dos cuadros de texto para el nombre de usuario y la contraseña, un Recordarme checkbox y un botón de inicio de sesión (consulte la figura 1). El código valida las credenciales introducidas con una lista codificada de pares de nombre de usuario y contraseña (Scott/contraseña, Jisun y la contraseña y Sam y contraseña). En el <a id="Tutorial03"> </a> [ *configuración de autenticación de formularios y temas avanzados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) tutorial hemos actualizado el código de la página de inicio de sesión para almacenar información adicional en los formularios vale de autenticación `UserData` propiedad.


[![Interfaz de la página de inicio de sesión incluye dos cuadros de texto, un control CheckBoxList y un botón](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Figura 1**: interfaz incluye dos cuadros de texto de la página de inicio de sesión la, un control CheckBoxList y un botón ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


Interfaz de usuario de la página de inicio de sesión puede permanecer sin cambios, pero tenemos que reemplazar el botón de inicio de sesión `Click` controlador de eventos con código que valida el usuario en el almacén de usuario del marco de pertenencia. Actualice el controlador de eventos para que su código aparece como sigue:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Este código es sorprendentemente sencillo. Comenzamos llamando el `Membership.ValidateUser` método, pasando el nombre de usuario proporcionado y la contraseña. Si el método devuelve true, el usuario ha iniciado sesión en el sitio a través de la `FormsAuthentication` método RedirectFromLoginPage de la clase. (Como se explicó en la <a id="Tutorial2"> </a> [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, el `FormsAuthentication.RedirectFromLoginPage` crea los formularios de vale de autenticación y, a continuación, redirige al usuario a la página adecuada.) Si las credenciales no son válidas, sin embargo, la `InvalidCredentialsMessage` etiqueta se muestra para informar al usuario que su nombre de usuario o contraseña era incorrecta.

Eso es todo esto!

Para comprobar que la página de inicio de sesión funciona según lo esperado, intente iniciar sesión con una de las cuentas de usuario que creó en el tutorial anterior. O bien, si aún no ha creado una cuenta, siga adelante y crear uno desde el `~/Membership/CreatingUserAccounts.aspx` página.

> [!NOTE]
> Cuando el usuario escribe sus credenciales y envía el formulario de la página de inicio de sesión, las credenciales, incluida su contraseña, se transmiten a través de Internet al servidor web en *texto sin formato*. Esto significa que los hackers examen el tráfico de red pueden ver el nombre de usuario y la contraseña. Para evitar esto, es esencial para cifrar el tráfico de red mediante [capas de sockets seguros (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Esto garantizará que se cifra las credenciales (así como marcado HTML de la página completa) desde el momento en que dejan el explorador hasta que se reciben por el servidor web.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Cómo el marco de pertenencia controla los intentos de inicio de sesión no válido

Cuando un visitante llega a la página de inicio de sesión y envía sus credenciales, el explorador realiza una solicitud HTTP a la página de inicio de sesión. Si las credenciales son válidas, la respuesta HTTP incluye el vale de autenticación en una cookie. Por lo tanto, un hacker que intente irrumpir en el sitio podría crear un programa que exhaustivamente envía solicitudes HTTP a la página de inicio de sesión con un nombre de usuario válido y una estimación de la contraseña. Si el intento de contraseña es correcto, la página de inicio de sesión devolverá la cookie de vale de autenticación, momento en que el programa sabe que ha tropezado un par de nombre de usuario y contraseña válidos. A través de fuerza bruta, este tipo de programa puede ser capaz de tropieza con una contraseña de usuario, especialmente si la contraseña es débil.

Para evitar estos ataques de fuerza bruta, el marco de pertenencia se bloquea un usuario si hay un cierto número de intentos de inicio de sesión incorrecto en un determinado periodo de tiempo. Los parámetros exactos son configurables a través de los dos valores de configuración de proveedor de pertenencia:

- `maxInvalidPasswordAttempts` -Especifica cuántas contraseñas no válidas se permiten los intentos del usuario dentro del período de tiempo antes de que la cuenta está bloqueada. El valor predeterminado es 5.
- `passwordAttemptWindow` -indica el período de tiempo en minutos durante el cual el número especificado de intentos de inicio de sesión no válido hará que la cuenta que se bloquee. El valor predeterminado es 10.

Si un usuario se ha bloqueado, no puede iniciar sesión hasta que un administrador la desbloquee su cuenta de usuario. Cuando un usuario está bloqueado, el `ValidateUser` método *siempre* devolver `false`, incluso si se han proporcionado credenciales válidas. Aunque este comportamiento reduce la probabilidad de que un hacker se interrumpirá en el sitio a través de métodos de fuerza bruta, puede terminar el bloqueo de un usuario válido que simplemente ha olvidado su contraseña o accidentalmente el BLOQ o es tener un mal día de escritura.

Lamentablemente, no hay ninguna herramienta integrada para desbloquear una cuenta de usuario. Para desbloquear una cuenta, puede modificar la base de datos directamente - cambiar el `IsLockedOut` campo el `aspnet_Membership` de tabla para la cuenta de usuario correspondiente, o crear una basada en web interfaz que se enumera bloqueada cuentas con las opciones para desbloquearlos. Examinaremos crear interfaces administrativas para llevar a cabo tareas relacionadas con el rol y la cuenta de usuario comunes en un futuro tutorial.

> [!NOTE]
> Una desventaja de la `ValidateUser` método es que cuando las credenciales proporcionadas no son válidas, no proporciona ninguna explicación sobre el motivo por. Las credenciales pueden ser no válidas porque no hay ningún par de nombre de usuario y contraseña correspondiente en el almacén de usuario, o porque el usuario no han sido aprobado o porque se ha bloqueado el usuario. En el paso 4, veremos cómo mostrar un mensaje más detallado al usuario cuando se produce un error en su intento de inicio de sesión.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Paso 2: Recopilar las credenciales a través del Control de inicio de sesión Web

El [control de inicio de sesión Web](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) representa una interfaz de usuario predeterminada muy similar al que se creó en el <a id="SKM5"> </a> [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial. Mediante el control de inicio de sesión, nos ahorra el trabajo de tener que crear la interfaz para recopilar las credenciales de s visitante. Además, el control de inicio de sesión inicia automáticamente la sesión del usuario (suponiendo que las credenciales enviadas son válidas), con lo que guardar nos de tener que escribir ningún código.

Vamos a actualizar `Login.aspx`, reemplazando la interfaz creada manualmente y de código con un control de inicio de sesión. Empiece quitando el marcado existente y código en `Login.aspx`. Puede eliminar directamente, o simplemente coméntelo. Para comentar el marcado declarativo, inclúyalo con la `<%--` y `--%>` delimitadores. Puede escribir estos delimitadores manualmente, o bien, como se muestra en la figura 2, puede seleccionar el texto que se marque como comentario y, a continuación, haga clic en el comentario en el icono de las líneas seleccionadas en la barra de herramientas. De forma similar, puede usar el comentario en el icono de las líneas seleccionadas para marcar como comentario el código seleccionado en la clase de código subyacente.


[![Comente el marcado declarativo existente y código fuente en Login.aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Figura 2**: comentario Out the existente marcado declarativo y código fuente en `Login.aspx` ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> El comentario en el icono de líneas seleccionado no está disponible al ver el marcado declarativo en Visual Studio 2005. Si usa Visual Studio 2008 no necesita agregar manualmente el `<%--` y `--%>` delimitadores.


A continuación, arrastre un control de inicio de sesión desde el cuadro de herramientas en la página y establezca su `ID` propiedad `myLogin`. En este momento, la pantalla debe ser similar a la figura 3. Tenga en cuenta que la interfaz predeterminada del control de inicio de sesión incluye controles de cuadro de texto para el nombre de usuario y contraseña, un Recordármelo la próxima vez casilla de verificación y un botón de registro. También hay `RequiredFieldValidator` controles para los dos cuadros de texto.


[![Agregar un Control de inicio de sesión a la página](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Figura 3**: agregar un Control de inicio de sesión a la página ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


Y listo! Cuando se hace clic en el botón Iniciar sesión del control de inicio de sesión, se producirá una devolución de datos y el control de inicio de sesión llamará el `Membership.ValidateUser` método, pasando el nombre de usuario especificado y la contraseña. Si las credenciales son válidas, el control de inicio de sesión muestra un mensaje con esta. Si, sin embargo, las credenciales son válidas, el control de inicio de sesión crea los formularios de vale de autenticación y redirige al usuario a la página adecuada.

El control de inicio de sesión usa cuatro factores para determinar la página apropiada para redirigir al usuario cuando inician una sesión correctamente:

- Si el control de inicio de sesión está en la página de inicio de sesión tal como se define por `loginUrl` en la configuración de autenticación de formularios; este valor de configuración predeterminado es `Login.aspx`
- La presencia de un `ReturnUrl` parámetro querystring
- El valor del control de inicio de sesión [ `DestinationUrl` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- El `defaultUrl` valor especificado en los formularios de opciones de configuración de autenticación; este valor de configuración predeterminado es `Default.aspx`

Figura 4 se muestra cómo el control de inicio de sesión utiliza estos cuatro parámetros para llegar a su decisión de página correspondiente.


[![Agregar un Control de inicio de sesión a la página](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Figura 4**: agregar un Control de inicio de sesión a la página ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


Dedique un momento para probar el control de inicio de sesión por visitar el sitio a través de un explorador e iniciando sesión como un usuario existente en el marco de pertenencia.

Interfaz representado del control de inicio de sesión es altamente configurable. Hay una serie de propiedades que afectan a su apariencia; Además, el control de inicio de sesión se puede convertir en una plantilla para un control preciso sobre el diseño de los elementos de interfaz de usuario. El resto de este paso examina cómo personalizar la apariencia y el diseño.

### <a name="customizing-the-login-controls-appearance"></a>Personalizar la apariencia del Control de inicio de sesión

Configuración de propiedades del control de inicio de sesión predeterminada presentar una interfaz de usuario con un título (iniciar sesión), controles TextBox y etiqueta para las entradas de nombre de usuario y contraseña, un Recordarme la próxima vez CheckBox y un botón Iniciar sesión. La apariencia de estos elementos es todas configurable a través de numerosas propiedades del control de inicio de sesión. Además, se pueden agregar elementos de interfaz de usuario adicionales: por ejemplo, un vínculo a una página para crear una nueva cuenta de usuario - estableciendo una propiedad o dos.

Vamos a dedicar unos minutos en gussy el aspecto de nuestro control de inicio de sesión. Puesto que la `Login.aspx` página ya tiene el texto en la parte superior de la página que indica el inicio de sesión, el título del control de inicio de sesión es superflua. Por lo tanto, borre el [ `TitleText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) valor con el fin de quitar el título del control de inicio de sesión.

El nombre de usuario: y la contraseña: las etiquetas a la izquierda de los dos controles TextBox se pueden personalizar mediante la [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) y [ `PasswordLabelText` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), respectivamente. Vamos a cambiar el nombre de usuario: etiqueta para leer el nombre de usuario:. Los estilos de etiqueta y el cuadro de texto son configurables a través de la [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) y [ `TextBoxStyle` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), respectivamente.

Recordar mi cuenta se puede establecer la propiedad de texto de la casilla de tiempo siguiente a través del control de inicio de sesión [ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), y su valor predeterminado comprueba el estado es configurable a través de la [ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (que el valor predeterminado es False). Avanzar y configurar el `RememberMeSet` se comprobará la propiedad en True para que el Recordarme la próxima vez casilla de verificación de forma predeterminada.

El control de inicio de sesión ofrece dos propiedades para ajustar el diseño de sus controles de interfaz de usuario. El [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indica si el nombre de usuario: y la contraseña: las etiquetas que aparecen a la izquierda de los cuadros de texto correspondiente (el valor predeterminado) o por encima de ellos. El [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indica si las entradas de nombre de usuario y contraseña estén situadas verticalmente (uno encima del otro) o en horizontal. Voy a dejar estas dos propiedades con sus valores predeterminados, pero le animo a probar la configuración de estas dos propiedades a sus valores no predeterminados para ver el efecto resultante.

> [!NOTE]
> En la sección siguiente, la configuración de diseño del Control de inicio de sesión, veremos con plantillas para definir el diseño concreto de elementos de interfaz de usuario del control de diseño.


¿Ajustar los valores de propiedad del control de inicio de sesión estableciendo la [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) y [ `CreateUserUrl` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) a no registrado aún? Cree una cuenta. y `~/Membership/CreatingUserAccounts.aspx`, respectivamente. Esto agrega un hipervínculo a la interfaz del control de inicio de sesión que apunta a la página se creó en el <a id="SKM6"> </a> [tutorial anterior](creating-user-accounts-cs.md). El control de inicio de sesión [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) y [ `HelpPageUrl` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) y [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) y [ `PasswordRecoveryUrl` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) profesional de la misma manera, los vínculos a una página de ayuda y una página de recuperación de contraseña.

Después de realizar estos cambios de propiedad, deben ser similares al que se muestra en la figura 5 marcado declarativo y la apariencia del control de inicio de sesión.


[![Valores de las propiedades del Control de inicio de sesión determinan su apariencia](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Figura 5**: valores determinan su apariencia las propiedades del Control del inicio de sesión ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Configuración de diseño del Control de inicio de sesión

Interfaz de usuario predeterminada del control de inicio de sesión Web presenta la interfaz en un elemento HTML `<table>`. Pero, ¿qué ocurre si se necesita un mayor control sobre la salida representada? Puede que queramos reemplazar el `<table>` con una serie de `<div>` etiquetas. O bien, ¿qué sucede si nuestra aplicación requiere credenciales adicionales para la autenticación? Muchos sitios Web financieros, por ejemplo, exigen que los usuarios proporcionar no solo un nombre de usuario y contraseña, pero también un número de identificación Personal (PIN) u otra información de identificación. Todo lo que pueden ser los motivos, es posible convertir el control de inicio de sesión en una plantilla, desde el que podemos definir explícitamente marcado declarativo de la interfaz.

Es necesario hacer dos cosas con el fin de actualizar el control de inicio de sesión para recopilar credenciales adicionales:

1. Actualizar la interfaz del control de inicio de sesión para incluir controles Web para recopilar las credenciales adicionales.
2. Invalidar la lógica de autenticación interno del control de inicio de sesión para que un usuario se autentica solo si su nombre de usuario y la contraseña es válida y sus credenciales adicionales son válidos, demasiado.

Para lograr la primera tarea, se debe convertir el control de inicio de sesión en una plantilla y agregue los controles Web necesarios. En cuanto a la segunda tarea, se puede sustituir la lógica de autenticación del control de inicio de sesión mediante la creación de un controlador de eventos para el control [ `Authenticate` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Vamos a actualizar el control de inicio de sesión para que se solicita a los usuarios su nombre de usuario, contraseña y dirección de correo electrónico y sólo autentica al usuario si la dirección de correo electrónico proporcionada coincide con su dirección de correo electrónico en el archivo. En primer lugar, necesitamos convertir la interfaz del control de inicio de sesión en una plantilla. Desde la etiqueta inteligente del control de inicio de sesión, elija a la opción convertir en plantilla.


[![Convertir el Control de inicio de sesión en una plantilla](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Figura 6**: convertir el Control de inicio de sesión en una plantilla ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> Para revertir el control de inicio de sesión a su versión template previamente, haga clic en el vínculo de restablecimiento de la etiqueta inteligente del control.


Convertir el control de inicio de sesión a una plantilla agrega un `LayoutTemplate` al código de marcado declarativo con elementos HTML y controles Web define la interfaz de usuario del control. Como se muestra en la figura 7, convertir el control a una plantilla quita un número de propiedades de la ventana Propiedades, como `TitleText`, `CreateUserUrl`, y así sucesivamente, ya que estos valores de propiedad se omiten cuando se usa una plantilla.


[![Menos propiedades están que disponibles cuando el Control Login se convierte en una plantilla](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Figura 7**: menos propiedades están disponibles cuando el Control Login se convierte en una plantilla ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


El marcado HTML en el `LayoutTemplate` se puede modificar según sea necesario. Del mismo modo, no dude en agregar los nuevos controles Web a la plantilla. Sin embargo, es importante los controles Web de núcleo de ese control de inicio de sesión permanecen en la plantilla y mantener sus asignado `ID` valores. En concreto, no quite ni cambie el nombre de la `UserName` o `Password` cuadros de texto, el `RememberMe` casilla, la `LoginButton` botón, el `FailureText` etiqueta, o el `RequiredFieldValidator` controles.

Para recopilar la dirección de correo electrónico del visitante, necesitamos agregar un cuadro de texto a la plantilla. Agregue el siguiente marcado declarativo entre la fila de tabla (`<tr>`) que contiene el `Password` casilla de verificación de tiempo de cuadro de texto y la fila de tabla que contiene el recordar mi cuenta a continuación:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Después de agregar el `Email` cuadro de texto, visite la página a través de un explorador. Como se muestra en la figura 8, interfaz de usuario del control de inicio de sesión incluye ahora un tercer cuadro de texto.


[![El Control de inicio de sesión ahora incluye un cuadro de texto dirección de correo electrónico del usuario](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Figura 8**: el Control de inicio de sesión ahora incluye un cuadro de texto dirección de correo electrónico del usuario ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


En este momento, el control de inicio de sesión todavía está usando el `Membership.ValidateUser` método para validar las credenciales proporcionadas. En consecuencia, el valor introducido en el `Email` cuadro de texto no influye en si el usuario puede iniciar sesión. En el paso 3 veremos cómo invalidar la lógica de autenticación del control de inicio de sesión para que las credenciales solo se consideran válidas si el nombre de usuario y la contraseña son válidos y la dirección de correo electrónico especificada coincide con la dirección de correo electrónico en el archivo.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Paso 3: Modificar la lógica de autenticación de inicio de sesión del Control

Cuando un visitante le proporciona las credenciales y hace clic en el botón Iniciar sesión, una devolución de datos que habrá trastornos y el inicio de sesión controlan progresa a través de su flujo de trabajo de autenticación. El flujo de trabajo se inicia al elevar la [ `LoggingIn` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Los controladores de eventos asociados a este evento pueden cancelar el registro de la operación estableciendo el `e.Cancel` propiedad `true`.

Si no se cancela el registro de la operación, el flujo de trabajo progresa generando la [ `Authenticate` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Si hay un controlador de eventos para el `Authenticate` eventos, es responsable de determinar si las credenciales proporcionadas son válidas o no. Si no se especifica ningún controlador de eventos, el control de inicio de sesión utiliza el `Membership.ValidateUser` método para determinar la validez de las credenciales.

Si las credenciales proporcionadas son válidas, a continuación, se crea el vale de autenticación de formularios, el [ `LoggedIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) se genera, y se redirige al usuario a la página adecuada. Si, sin embargo, las credenciales se consideran no válidas, la [ `LoginError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) se genera y se muestra un mensaje que informa al usuario sus credenciales no eran válidas. De forma predeterminada, un error en el inicio de sesión de control simplemente establece su `FailureText` etiqueta de propiedad de texto del control a un mensaje de error (el intento de inicio de sesión no era correcta. Vuelva a intentarlo). Sin embargo, si el control de inicio de sesión [ `FailureAction` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) está establecido en `RedirectToLoginPage`, a continuación, los problemas de control de inicio de sesión una `Response.Redirect` a la página de inicio de sesión que anexar el parámetro querystring `loginfailure=1` (lo que hace que el inicio de sesión control para mostrar el mensaje de error).

Figura 9 ofrece un diagrama de flujo del flujo de trabajo de autenticación.


[![Flujo de trabajo de autenticación de inicio de sesión del Control](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Figura 9**: flujo de trabajo de autenticación del Control de inicio de sesión ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> Si sabe cuándo utilizaría la `FailureAction`del `RedirectToLogin` opción de la página, considere el siguiente escenario. Ahora nuestra `Site.master` página maestra tiene actualmente el texto Hola stranger se muestran en la columna izquierda cuando visita un usuario anónimo, pero imaginemos que deseamos reemplazar el texto con un control de inicio de sesión. Esto permitiría que un usuario anónimo iniciar sesión desde cualquier página en el sitio, en lugar de requerir que visite directamente la página de inicio de sesión. Sin embargo, si un usuario no pudo iniciar sesión a través del control de inicio de sesión representado por la página maestra, tendría sentido redirigirlas a la página de inicio de sesión (`Login.aspx`) porque esa página probable incluye instrucciones adicionales, vínculos y otro ayuda - como vínculos para crear un nueva cuenta o recuperar una contraseña perdida - que no se han agregado a la página maestra.


### <a name="creating-theauthenticateevent-handler"></a>Crear el`Authenticate`controlador de eventos

Para conectar nuestra lógica de autenticación personalizada, es necesario crear un controlador de eventos para el control de inicio de sesión `Authenticate` eventos. Creación de un controlador de eventos para el `Authenticate` eventos generará la siguiente definición de controlador de eventos:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Como puede ver, el `Authenticate` controlador de eventos se pasa un objeto de tipo [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) como su segundo parámetro de entrada. El `AuthenticateEventArgs` clase contiene una propiedad booleana denominada `Authenticated` que se utiliza para especificar si las credenciales proporcionadas son válidas. Nuestra tarea, a continuación, es escribir código aquí que determina si las credenciales proporcionadas son válidas o no y para establecer el `e.Authenticate` propiedad según corresponda.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinar y validar las credenciales proporcionadas

Use el control de inicio de sesión [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) y [ `Password` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) para determinar las credenciales de usuario y la contraseña escritas por el usuario. Con el fin de determinar los valores especificados en los controles Web adicionales (como el `Email` cuadro de texto se agregó en el paso anterior), utilice *`LoginControlID`* `.FindControl`("*`controlID`*") para obtener una programación referencia al control Web en la plantilla cuya `ID` es igual a la propiedad *`controlID`*. Por ejemplo, para obtener una referencia a la `Email` cuadro de texto, use el código siguiente:

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Para validar las credenciales del usuario, es necesario hacer dos cosas:

1. Asegúrese de que el nombre de usuario y la contraseña son válidos
2. Asegúrese de que dirección de correo electrónico que escribió coincide con la dirección de correo electrónico registrada para el usuario intenta iniciar sesión

Para realizar la primera comprobación que podemos usar simplemente el `Membership.ValidateUser` método como observamos en el paso 1. Para la comprobación de segundo, debemos determinar la dirección de correo electrónico del usuario para que nos podemos comparar a la dirección de correo electrónico que escriben en el control de cuadro de texto. Para obtener información acerca de un usuario determinado, use el `Membership` la clase [ `GetUser` método](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

El `GetUser` método tiene un número de sobrecargas. Si se usa sin pasar ningún parámetro, devuelve información sobre el usuario con sesión iniciada actualmente. Para obtener información acerca de un usuario determinado, llame a `GetUser` pasando su nombre de usuario. En cualquier caso, `GetUser` devuelve un [ `MembershipUser` objeto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que tiene propiedades como `UserName`, `Email`, `IsApproved`, `IsOnline`, y así sucesivamente.

El código siguiente implementa estas comprobaciones de dos. Si, a continuación, ambos pasan `e.Authenticate` está establecido en `true`, en caso contrario, se le asigna `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Con este código en su lugar, intente iniciar la sesión como usuario válido, especifique el nombre de usuario correcto, la contraseña y dirección de correo electrónico. Inténtelo de nuevo, pero esta vez utilice intencionadamente una dirección de correo electrónico incorrecta (consulte la figura 10). Por último, pruebe una tercera vez con un nombre de usuario inexistente. En el primer caso debe ser iniciado sesión correctamente en el sitio, pero en los dos últimos casos debería ver el mensaje de credenciales no válidas del control de inicio de sesión.


[![Tito no puede iniciar sesión cuando se suministra una dirección de correo electrónico incorrecta](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Figura 10**: Tito no registro en al proporcionar una dirección de correo electrónico incorrecta ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> Como se describe en la sección cómo la pertenencia Framework controla intentos no válidos inicio de sesión en el paso 1, cuando el `Membership.ValidateUser` método se llama y se pasan credenciales no válidas, realiza un seguimiento del intento de inicio de sesión no válido y el usuario se bloqueará si superan un determinado umbral de intentos no válidos dentro de un período de tiempo especificado. Desde las llamadas de lógica de autenticación personalizada el `ValidateUser` método, una contraseña incorrecta para un nombre de usuario válido incrementará el contador de intentos de inicio de sesión no válido, pero no, este contador se incrementa en el caso donde el nombre de usuario y la contraseña son válidos, pero el dirección de correo electrónico no es correcta. Lo más probable es que este comportamiento es adecuado, ya que es probable que un hacker se conoce el nombre de usuario y la contraseña, pero tiene que usar técnicas de fuerza bruta para determinar la dirección de correo electrónico del usuario.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Paso 4: Mejorar el mensaje de credenciales no válidas del Control de inicio de sesión

Cuando un usuario intenta iniciar sesión con credenciales no válidas, el control de inicio de sesión muestra un mensaje que explica que el intento de inicio de sesión no tuvo éxito. En concreto, el control muestra el mensaje especificado por su [ `FailureText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), que tiene un valor predeterminado de su intento de inicio de sesión no fue correcta. Vuelva a intentarlo.

Recuerde que hay muchas razones por las credenciales de un usuario pueden no ser válidas:

- El nombre de usuario no exista.
- El nombre de usuario existe, pero la contraseña no es válida
- El nombre de usuario y la contraseña son válidos, pero el usuario aún no está aprobado
- El nombre de usuario y la contraseña son válidos, pero el usuario queda bloqueado fuera (probablemente porque superó el número de intentos de inicio de sesión no válido en el período de tiempo especificado)

Y puede haber otros motivos cuando se usa la lógica de autenticación personalizada. Por ejemplo, con el código se escribió en el paso 3, el nombre de usuario y contraseña sea válida, pero la dirección de correo electrónico sea correcta.

Independientemente de por qué las credenciales son válidas, el control de inicio de sesión muestra el mismo mensaje de error. Esta falta de comentarios puede resultar confusa para un usuario cuya cuenta aún no se ha aprobado o que se ha bloqueado. Con un poco de trabajo, sin embargo, podemos tenemos el control de inicio de sesión mostrará un mensaje más apropiado.

Cada vez que un usuario intenta iniciar sesión con credenciales no válidas, el control de inicio de sesión provoca su `LoginError` eventos. Siga adelante y crear un controlador de eventos para este evento y agregue el código siguiente:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

El código anterior se inicia mediante el establecimiento del control de inicio de sesión `FailureText` propiedad en el valor predeterminado (el intento de inicio de sesión no era correcta. Vuelva a intentarlo). A continuación, comprueba si el nombre de usuario proporcionado se asigna a una cuenta de usuario existente. Si es así, consulta resultante `MembershipUser` del objeto `IsLockedOut` y `IsApproved` propiedades para determinar si la cuenta se ha bloqueado o que no han sido aprobada. En cualquier caso, el `FailureText` se actualiza la propiedad a un valor correspondiente.

Para probar este código, intencionadamente intenta iniciar sesión como un usuario existente, pero utiliza una contraseña incorrecta. Hacer esto cinco veces en una fila dentro de un período de tiempo de 10 minutos y se bloqueará la cuenta. Como se muestra en la figura 11, inicio de sesión subsiguiente intentos siempre se producirá un error (incluso con la contraseña correcta), pero tendrán ahora más descriptivo para mostrar su cuenta se bloqueó debido a demasiados intentos de inicio de sesión no válido. Póngase en contacto con el administrador para que el mensaje de cuenta desbloqueada.


[![Tito realiza demasiados intentos de inicio de sesión no válido y se ha bloqueado](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Figura 11**: Tito realiza demasiado muchos intentos no válidos de inicio de sesión y ha sido bloqueada ([haga clic aquí para ver imagen en tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>Resumen

Anterior en este tutorial, nuestra página de inicio de sesión valida las credenciales proporcionadas con una lista codificada de pares de nombre de usuario y contraseña. En este tutorial, se actualiza la página para validar las credenciales con el marco de pertenencia. En el paso 1 contemplamos utilizando el `Membership.ValidateUser` método mediante programación. En el paso 2 reemplazamos nuestra interfaz de usuario creada manualmente y el código con el control de inicio de sesión.

El control de inicio de sesión representa una interfaz de usuario de inicio de sesión estándar y valida automáticamente las credenciales del usuario en el marco de pertenencia. Además, en el caso de credenciales válidas, el control de inicio de sesión inicia sesión el usuario en mediante la autenticación de formularios. En resumen, una experiencia de usuario de inicio de sesión completamente funcional está disponible, simplemente arrastre el control de inicio de sesión a una página, ningún marcado declarativo adicional o el código necesario. ¿Qué es más, el control de inicio de sesión es muy personalizable, lo que permite un elevado grado de control sobre ambos la lógica de autenticación y la interfaz de usuario representado.

En este momento pueden crear una nueva cuenta de usuario y el registro de visitantes de nuestro sitio Web en el sitio, pero aún tenemos que mirar restringir el acceso a las páginas según el usuario autenticado. Actualmente, cualquier usuario autenticado o anónimo, puede ver cualquier página en nuestro sitio. Además de controlar el acceso a las páginas de nuestro sitio según un usuario por usuario, tendremos que ciertas páginas cuya funcionalidad depende del usuario. El siguiente tutorial examina cómo limitar el acceso y la funcionalidad en la página según el usuario ha iniciado sesión.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Mostrar mensajes personalizados a bloqueada y que los usuarios no aprobado](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Examen de ASP.NET del 2.0 perfil, Roles y pertenencia](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Cómo: Crear una página de inicio de sesión ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentación técnica de Control de inicio de sesión](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Uso de los controles de inicio de sesión](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Teresa Murphy y Michael Olivero. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-user-accounts-cs.md)
> [Siguiente](user-based-authorization-cs.md)
