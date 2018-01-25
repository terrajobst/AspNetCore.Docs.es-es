---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
title: "Validando las credenciales de usuario en el almacén de usuario de pertenencia (VB) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial, examinaremos cómo validar las credenciales del usuario en el almacén de usuario de pertenencia mediante medios mediante programación y el control de inicio de sesión..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 17772912-b47b-4557-9ce9-80f22df642f7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
msc.type: authoredcontent
ms.openlocfilehash: f57bc8c32757c1ea25bf6bbb34539570e4c09aad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="validating-user-credentials-against-the-membership-user-store-vb"></a>Validando las credenciales de usuario en el almacén de usuario de pertenencia (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_vb.pdf)

> En este tutorial, examinaremos cómo validar las credenciales del usuario en el almacén de usuario de pertenencia mediante medios mediante programación y el control de inicio de sesión. También veremos cómo personalizar la apariencia y el comportamiento del control de inicio de sesión.


## <a name="introduction"></a>Introducción

En el <a id="Tutorial05"> </a> [tutorial anterior](creating-user-accounts-vb.md) explicamos cómo crear una nueva cuenta de usuario en el marco de trabajo de pertenencia. Primero analizamos crear mediante programación las cuentas de usuario a través de la `Membership` la clase `CreateUser` (método) y, a continuación, examinar utilizando el control CreateUserWizard Web. Sin embargo, la página de inicio de sesión actualmente valida las credenciales proporcionadas con una lista de pares de nombre de usuario y una contraseña codificados de forma rígida. Es necesario actualizar la lógica de la página de inicio de sesión para que valida las credenciales con el almacén de usuario de la pertenencia del marco de trabajo.

Mucho al igual que con la creación de cuentas de usuario, las credenciales se pueden validar mediante programación o mediante declaración. La API de pertenencia incluye un método para validar mediante programación las credenciales del usuario en el almacén de usuario. Y ASP.NET se distribuye con el control de inicio de sesión Web, que representa una interfaz de usuario con cuadros de texto para el nombre de usuario y contraseña y un botón para iniciar sesión.

En este tutorial, examinaremos cómo validar las credenciales del usuario en el almacén de usuario de pertenencia mediante medios mediante programación y el control de inicio de sesión. También veremos cómo personalizar la apariencia y el comportamiento del control de inicio de sesión. Comencemos.

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Paso 1: Validar las credenciales en el almacén de usuario de pertenencia

Para los sitios web que utilizan la autenticación de formularios, un usuario inicia sesión el sitio Web al visitar una página de inicio de sesión y especificar sus credenciales. Estas credenciales, a continuación, se comparan con el almacén del usuario. Si son válidos, se concede al usuario un vale de autenticación de formularios, que es un token de seguridad que indica la identidad y la autenticidad del visitante.

Para validar un usuario con el marco de trabajo de pertenencia, utilice el `Membership` la clase [ `ValidateUser` método](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). El `ValidateUser` método toma dos parámetros de entrada - `username` y `password` - y devuelve un valor booleano que indica si las credenciales son válidas. Al igual que con la `CreateUser` método se examina en el tutorial anterior, el `ValidateUser` método delega la validación real al proveedor de pertenencia configurado.

El `SqlMembershipProvider` valida las credenciales proporcionadas al obtener la contraseña del usuario especificado a través de la `aspnet_Membership_GetPasswordWithFormat` procedimiento almacenado. Recuerde que el `SqlMembershipProvider` almacena las contraseñas de los usuarios mediante uno de estos tres formatos: borrar, cifrado, o aplicar un algoritmo hash. El `aspnet_Membership_GetPasswordWithFormat` procedimiento almacenado devuelve la contraseña en su formato sin procesar. Para las contraseñas cifradas o algoritmo hash, el `SqlMembershipProvider` transforma el `password` valor pasado en el `ValidateUser` método en su equivalente cifrados o aplica un algoritmo hash de estado y, a continuación, se compara con lo que se devuelve desde la base de datos. Si la contraseña almacenada en la base de datos coincide con la contraseña con formato especificada por el usuario, las credenciales son válidas.

Vamos a actualizar nuestra página de inicio de sesión (~ /`Login.aspx`) para que valida las credenciales proporcionadas en el almacén de usuario de pertenencia al marco de trabajo. Hemos creado esta página de inicio de sesión en la <a id="Tutorial02"> </a> [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial, crear una interfaz con dos cuadros de texto para el nombre de usuario y la contraseña, una Recordarme casilla de verificación y un botón de inicio de sesión (consulte la figura 1). El código valida las credenciales introducidas en una lista codificada de forma rígida de pares de nombre de usuario y contraseña (Scott/contraseña, Jisun/contraseña y Sam/contraseña). En el <a id="Tutorial03"> </a> [ *configuración de autenticación de formularios y temas avanzados* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) tutorial actualizamos código de la página de inicio de sesión para almacenar información adicional en los formularios vale de autenticación `UserData` propiedad.


[![Interfaz de la página de inicio de sesión incluye dos cuadros de texto, un control CheckBoxList y un botón](validating-user-credentials-against-the-membership-user-store-vb/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image1.png)

**Figura 1**: interfaz incluye dos cuadros de texto de la página de inicio de sesión la, un control CheckBoxList y un botón ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image3.png))


Interfaz de usuario de la página de inicio de sesión puede permanecer sin cambios, pero es necesario reemplazar el botón de inicio de sesión `Click` controlador de eventos con el código que valida el usuario en el almacén de usuario de pertenencia al marco de trabajo. Actualice el controlador de eventos para que su código aparezca como sigue:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample1.vb)]

Este código es sumamente simple. Comenzamos mediante una llamada a la `Membership.ValidateUser` método, pasando el nombre de usuario proporcionado y la contraseña. Si dicho método devuelva True, el usuario ha iniciado sesión en el sitio a través de la `FormsAuthentication` método RedirectFromLoginPage de la clase. (Como se explicó en la <a id="Tutorial02"> </a> [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial, el `FormsAuthentication.RedirectFromLoginPage` crean los formularios de vale de autenticación y, a continuación, redirige al usuario a la página adecuada.) Si las credenciales son válidas, sin embargo, la `InvalidCredentialsMessage` etiqueta se muestra, que informa al usuario que el nombre de usuario o la contraseña es incorrecta.

Eso es todo lo!

Para comprobar que la página de inicio de sesión funciona según lo esperado, intente iniciar sesión con una de las cuentas de usuario que creó en el tutorial anterior. O bien, si aún no ha creado una cuenta, continúe y crear uno a partir del `~/Membership/CreatingUserAccounts.aspx` página.

> [!NOTE]
> Cuando el usuario escribe sus credenciales y envía el formulario de la página de inicio de sesión, las credenciales, incluida su contraseña, se transmiten a través de Internet al servidor web en *texto sin formato*. Esto significa que los piratas informáticos examen el tráfico de red pueden ver el nombre de usuario y la contraseña. Para evitar esto, es esencial para cifrar el tráfico de red mediante el uso de [capas de sockets seguros (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Esto garantizará que se cifra las credenciales (así como marcado HTML de la página completa) desde el momento en que dejan el explorador hasta que se reciben en el servidor web.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Cómo el marco de trabajo de pertenencia controla los intentos de inicio de sesión no válido

Cuando un visitante llega a la página de inicio de sesión y envía sus credenciales, el explorador realiza una solicitud HTTP a la página de inicio de sesión. Si las credenciales son válidas, la respuesta HTTP incluye el vale de autenticación en una cookie. Por lo tanto, un pirata informático intentando obtener acceso en el sitio podría crear un programa que exhaustivamente envía las solicitudes HTTP a la página de inicio de sesión con un nombre de usuario válido y una estimación en la contraseña. Si la estimación de la contraseña es correcta, la página de inicio de sesión devolverá la cookie de vale de autenticación, momento en que el programa sepa que se encuentra un par de nombre de usuario y contraseña válidos. Por fuerza bruta, este tipo de programa podrían llevar a que se encuentre en la contraseña de un usuario, especialmente si la contraseña es débil.

Para evitar estos ataques por fuerza bruta, el marco de trabajo de pertenencia se bloquea un usuario si hay un determinado número de intentos de inicio de sesión incorrecto en un período de tiempo determinado. Los parámetros exactos que son configurables a través de los dos valores de configuración de proveedor de pertenencia:

- `maxInvalidPasswordAttempts`-Especifica cuántas contraseñas no válidas se permiten los intentos del usuario dentro del período de tiempo antes de que la cuenta está bloqueada. El valor predeterminado es 5.
- `passwordAttemptWindow`-indica el período de tiempo en minutos durante los que el número especificado de intentos de inicio de sesión no válido hará que la cuenta que se bloquee. El valor predeterminado es 10.

Si un usuario se ha bloqueado, no puede iniciar sesión hasta que un administrador desbloquee su cuenta de usuario. Cuando un usuario está bloqueado, el `ValidateUser` método *siempre* devolver `False`, incluso si se han proporcionado credenciales válidas. Aunque este comportamiento reduce la probabilidad de que un hacker se interrumpirá en el sitio a través de métodos de fuerza bruta, puede acabar bloquear un usuario válido que simplemente se ha olvidado su contraseña o accidentalmente tiene las teclas BLOQ MAYÚS o es tener un mal día de escritura.

Lamentablemente, no hay ninguna herramienta integrada para desbloquear una cuenta de usuario. Para desbloquear una cuenta, puede modificar la base de datos directamente - cambiar la `IsLockedOut` campo en el `aspnet_Membership` de la tabla de la cuenta de usuario correspondiente, o crear una basada en web interfaz que enumera bloqueada cuentas con opciones para desbloquearlos. Examinaremos crear interfaces administrativas para llevar a cabo tareas comunes de relacionada con el rol y la cuenta de usuario en un tutorial posterior.

> [!NOTE]
> Una desventaja de la `ValidateUser` método es que cuando las credenciales proporcionadas son válidas, no proporciona ninguna explicación en cuanto a por qué. Las credenciales pueden ser no válidas porque no hay ningún par de nombre de usuario y contraseña correspondiente en el almacén de usuario, o porque el usuario no han sido aprobado o porque se ha bloqueado el usuario. En el paso 4 veremos cómo mostrar un mensaje más detallado para el usuario cuando se produce un error en el intento de inicio de sesión.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Paso 2: Recopilar las credenciales a través del Control de Web de inicio de sesión

El [control de inicio de sesión Web](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) representa una interfaz de usuario predeterminada muy similar a la que se creó en el <a id="Tutorial02"> </a> [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial. Mediante el control de inicio de sesión, nos ahorra el trabajo de tener que crear la interfaz para recopilar las credenciales del visitante. Además, el control de inicio de sesión automáticamente inicia sesión en el usuario (suponiendo que las credenciales presentadas son válidas), con lo que se ahorra nos de tener que escribir ningún código.

Vamos a actualizar `Login.aspx`, reemplazando la interfaz creada manualmente y codificar con un control de inicio de sesión. Iniciar quitando el marcado existente y el código de `Login.aspx`. Puede eliminar directamente, o simplemente coméntelo. Para comentar marcado declarativo, insértelo con el `<%--` y `--%>` delimitadores. Puede escribir estos delimitadores manualmente, o bien, como se muestra en la figura 2, puede seleccionar el texto para convertir en comentario y, a continuación, haga clic en el comentario en la barra de herramientas en el icono de las líneas seleccionadas. De forma similar, puede usar el comentario el icono de las líneas seleccionadas para comentar el código seleccionado en la clase de código subyacente.


[![Comenta el marcado declarativo existente y el código fuente en Login.aspx](validating-user-credentials-against-the-membership-user-store-vb/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image4.png)

**Figura 2**: comentario fuera el existente marcado declarativo y el código de origen de Login.aspx ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image6.png))


> [!NOTE]
> El comentario el icono de líneas seleccionado no está disponible al ver el marcado declarativo en Visual Studio 2005. Si no usa Visual Studio 2008 debe agregar manualmente el `<%--` y `--%>` delimitadores.


A continuación, arrastre un control de inicio de sesión desde el cuadro de herramientas en la página y establezca su `ID` propiedad `myLogin`. En este momento la pantalla debe ser similar a la figura 3. Tenga en cuenta que la interfaz predeterminada del control de inicio de sesión incluye controles de cuadro de texto para el nombre de usuario y contraseña, un Recordármelo la próxima vez que casilla de verificación y un botón de registro. También hay `RequiredFieldValidator` controles para los dos cuadros de texto.


[![Agregar un Control de inicio de sesión a la página](validating-user-credentials-against-the-membership-user-store-vb/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image7.png)

**Figura 3**: agregar un Control de inicio de sesión a la página ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image9.png))


Y acabamos! Cuando se hace clic en el botón Iniciar sesión del control de inicio de sesión, se producirá una devolución de datos y el control de inicio de sesión llamará el `Membership.ValidateUser` método, pasando el nombre de usuario especificado y la contraseña. Si las credenciales son válidas, el control de inicio de sesión muestra un mensaje que indica tal. Si, sin embargo, las credenciales son válidas, el control de inicio de sesión crea los formularios de vale de autenticación y redirige al usuario a la página apropiada.

El control de inicio de sesión utiliza cuatro factores para determinar la página correspondiente para redirigir al usuario cuando inician una sesión correctamente:

- Si el control de inicio de sesión está en la página de inicio de sesión definido por `loginUrl` en la configuración de autenticación de formularios; este valor de configuración predeterminado es`Login.aspx`
- La presencia de un `ReturnUrl` parámetro querystring
- El valor del control de inicio de sesión [ `DestinationUrl` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- El `defaultUrl` valor especificadas en los formularios de los valores de configuración de autenticación; este valor de configuración predeterminado es Default.aspx

Figura 4 muestra cómo el control de inicio de sesión utiliza estos cuatro parámetros para llegar a su decisión de página correspondiente.


[![Agregar un Control de inicio de sesión a la página](validating-user-credentials-against-the-membership-user-store-vb/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image10.png)

**Figura 4**: agregar un Control de inicio de sesión a la página ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image12.png))


Tómese un momento para probar el control de inicio de sesión por visitar el sitio a través de un explorador e iniciar sesión como un usuario existente en el marco de trabajo de pertenencia.

Interfaz representado del control de inicio de sesión es altamente configurable. Hay una serie de propiedades que afectan a su apariencia; Además, el control de inicio de sesión se puede convertir en una plantilla para un control preciso sobre el diseño de los elementos de la interfaz de usuario. El resto de este paso, examina cómo personalizar la apariencia y el diseño.

### <a name="customizing-the-login-controls-appearance"></a>Personalizar la apariencia del Control de inicio de sesión

Configuración de propiedades del control de inicio de sesión predeterminada representan una interfaz de usuario con un título (inicio de sesión), controles de cuadro de texto y la etiqueta para las entradas de nombre de usuario y una contraseña, un Recordarme la próxima vez CheckBox y un botón de inicio de sesión. Las repeticiones de estos elementos son todos los configurables a través de numerosas propiedades del control de inicio de sesión. Además, pueden agregarse elementos de la interfaz de usuario adicionales - como un vínculo a una página para crear una nueva cuenta de usuario - estableciendo una propiedad o dos.

Dediquemos unos minutos a gussy el aspecto de nuestro control de inicio de sesión. Puesto que la `Login.aspx` página ya tiene texto en la parte superior de la página que indica el inicio de sesión, el título del control de inicio de sesión es superflua. Por lo tanto, borrar el [ `TitleText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) valor con el fin de quitar el título del control de inicio de sesión.

El nombre de usuario: y la contraseña: se pueden personalizar las etiquetas a la izquierda de los dos controles de cuadro de texto a través de la [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) y [ `PasswordLabelText` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), respectivamente. Vamos a cambiar el nombre de usuario: etiqueta para leer el nombre de usuario:. Los estilos de etiqueta y cuadro de texto son configurables a través de la [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) y [ `TextBoxStyle` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), respectivamente.

Recordar mi cuenta se puede establecer la propiedad de texto de la casilla de tiempo siguiente mediante el control de inicio de sesión [ `RememberMeText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), y su valor predeterminado, se comprueba el estado es configurable a través de la [ `RememberMeSet` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx)(el valor predeterminado es False). Continúe y establezca el `RememberMeSet` propiedad en True para que recordar mi cuenta casilla la próxima vez que se va a comprobar de forma predeterminada.

El control de inicio de sesión ofrece dos propiedades para ajustar el diseño de sus controles de interfaz de usuario. El [ `TextLayout` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indica si el nombre de usuario: y la contraseña: las etiquetas que aparecen a la izquierda de los cuadros de texto correspondiente (el valor predeterminado) o por encima de ellos. El [ `Orientation` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indica si las entradas de nombre de usuario y contraseña están situadas verticalmente (uno encima de otro) o en horizontal. Voy a dejar estas dos propiedades establecidas en sus valores predeterminados, pero animamos a probar la configuración de estas dos propiedades con sus valores no predeterminados para ver el efecto resultante.

> [!NOTE]
> En la siguiente sección, configuración de diseño del Control de inicio de sesión, se consultará con plantillas para definir el diseño concreto de elementos del control de diseño de la interfaz de usuario.


¿Ajustar la configuración de propiedad del control de inicio de sesión estableciendo la [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) y [ `CreateUserUrl` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) a Not todavía registrado? Cree una cuenta. y `~/Membership/CreatingUserAccounts.aspx`, respectivamente. Esto agrega un hipervínculo a la interfaz del control de inicio de sesión que apunta a la página se creó en el <a id="Tutorial05"> </a> [tutorial anterior](creating-user-accounts-vb.md). El control de inicio de sesión [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) y [ `HelpPageUrl` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) y [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) y [ `PasswordRecoveryUrl` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) funcionan de la misma manera, representación de vínculos a una página de ayuda y una página de recuperación de contraseña.

Después de realizar estos cambios de propiedad, marcado declarativo y el aspecto de su control de inicio de sesión deben ser similares al que se muestra en la figura 5.


[![Valores de las propiedades del Control de inicio de sesión dictan su apariencia](validating-user-credentials-against-the-membership-user-store-vb/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image13.png)

**Figura 5**: valores dictar su apariencia las propiedades del Control del inicio de sesión ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Configuración de diseño del Control de inicio de sesión

Interfaz de usuario predeterminada del control de inicio de sesión Web distribuye la interfaz en un elemento HTML `<table>`. Pero ¿qué ocurre si se necesita un mayor control sobre la salida representada? Quizás desea reemplazar la `<table>` con una serie de `<div>` etiquetas. O bien, ¿qué ocurre si la aplicación requiere credenciales adicionales para la autenticación? Muchos sitios Web financieros, por ejemplo, los usuarios deben proporcionar no solo un nombre de usuario y contraseña, pero también un número de identificación Personal (NIP) y otra información de identificación. Lo que pueden ser los motivos, es posible convertir el control de inicio de sesión en una plantilla, desde el que podemos definir explícitamente marcado declarativo de la interfaz.

Debemos hacer dos cosas para actualizar el control de inicio de sesión para recopilar credenciales adicionales:

1. Actualizar la interfaz del control de inicio de sesión para incluir controles Web para recopilar las credenciales adicionales.
2. Invalide la lógica de autenticación interno del control de inicio de sesión para que un usuario se autentica solo si su nombre de usuario y la contraseña es válida y sus credenciales adicionales son válidos, demasiado.

Para realizar la primera tarea, es necesario convertir el control de inicio de sesión en una plantilla y agregar los controles Web necesarios. Como para la segunda tarea, se puede sustituir la lógica de autenticación del control de inicio de sesión mediante la creación de un controlador de eventos para el control [ `Authenticate` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Vamos a actualizar el control de inicio de sesión para que se solicita a los usuarios para su nombre de usuario, contraseña y dirección de correo electrónico y sólo autentica al usuario si la dirección de correo electrónico proporcionada coincide con su dirección de correo electrónico en el archivo. En primer lugar, necesitamos convertir interfaz del control de inicio de sesión en una plantilla. En la etiqueta inteligente del control de inicio de sesión, elija a la opción convertir en plantilla.


[![Convertir el Control de inicio de sesión en una plantilla](validating-user-credentials-against-the-membership-user-store-vb/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image16.png)

**Figura 6**: convertir el Control de inicio de sesión en una plantilla ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image18.png))


> [!NOTE]
> Para revertir el control de inicio de sesión con su versión template previamente, haga clic en el vínculo de restablecimiento de etiquetas inteligentes del control.


Convertir el control de inicio de sesión a una plantilla agrega un `LayoutTemplate` al código de marcado declarativo del control con los elementos HTML y controles Web definir la interfaz de usuario. Como se muestra en la figura 7, convertir el control a una plantilla de quita un número de propiedades de la ventana Propiedades, como `TitleText`, `CreateUserUrl`, y así sucesivamente, ya que estos valores de propiedad se omiten cuando se utiliza una plantilla.


[![Menos propiedades están que disponibles cuando el Control inicio de sesión se convierte en una plantilla](validating-user-credentials-against-the-membership-user-store-vb/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image19.png)

**Figura 7**: menos propiedades están disponibles cuando el Control inicio de sesión se convierte en una plantilla ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image21.png))


El marcado HTML en el `LayoutTemplate` puede modificarse según sea necesario. Del mismo modo, no dude en Agregar todos los controles Web nuevo a la plantilla. Sin embargo, es importante controles de Web principal de ese control de inicio de sesión permanecen en la plantilla y mantener tienen asignado `ID` valores. En concreto, no quite ni cambiar el nombre de la `UserName` o `Password` cuadros de texto, el `RememberMe` casilla de verificación, el `LoginButton` botón, el `FailureText` etiqueta, o la `RequiredFieldValidator` controles.

Para recopilar direcciones de correo electrónico del visitante, tenemos que agregar un cuadro de texto a la plantilla. Agregue el siguiente marcado declarativo entre la fila de tabla (`<tr>`) que contiene el `Password` casilla de verificación de tiempo de cuadro de texto y la fila de la tabla que contiene recordar mi cuenta a continuación:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample2.aspx)]

Después de agregar el `Email` cuadro de texto, visite la página a través de un explorador. Como se muestra en la figura 8, interfaz de usuario del control de inicio de sesión incluye ahora un tercer cuadro de texto.


[![El Control de inicio de sesión ahora incluye un cuadro de texto para la dirección de correo electrónico del usuario](validating-user-credentials-against-the-membership-user-store-vb/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image22.png)

**Figura 8**: el Control de inicio de sesión ahora incluye un cuadro de texto para la dirección de correo electrónico del usuario ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image24.png))


En este punto, el control de inicio de sesión sigue usando el `Membership.ValidateUser` método para validar las credenciales proporcionadas. En consecuencia, el valor introducido en el `Email` cuadro de texto no influye en si el usuario puede iniciar sesión. En el paso 3, veremos cómo invalidar la lógica de autenticación del control de inicio de sesión para que las credenciales sólo se consideran válidas si el nombre de usuario y la contraseña son válidos y la dirección de correo electrónico proporcionado coincide con la dirección de correo electrónico en el archivo.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Paso 3: Modificar la lógica de autenticación del Control de inicio de sesión

Cuando un visitante le proporciona las credenciales y hace clic en que el botón Iniciar sesión, una devolución de datos tiene lugar y el inicio de sesión de control progresa a través de su flujo de trabajo de autenticación. El flujo de trabajo se inicia al aumentar la [ `LoggingIn` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Los controladores de eventos asociados a este evento pueden cancelar la operación de inicio de sesión estableciendo la `e.Cancel` propiedad `True`.

Si no se cancela la operación de inicio de sesión, el flujo de trabajo progresa generando el [ `Authenticate` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Si no hay un controlador de eventos para el `Authenticate` eventos, es responsable de determinar si las credenciales proporcionadas son válidas o no. Si no se especifica ningún controlador de eventos, el control de inicio de sesión utiliza el `Membership.ValidateUser` método para determinar la validez de las credenciales.

Si las credenciales proporcionadas son válidas, se crea el vale de autenticación de formularios, el [ `LoggedIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) se genera, y se redirige al usuario a la página apropiada. Si, sin embargo, las credenciales se consideran no válidas, el [ `LoginError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) se genera y se muestra un mensaje que informa al usuario que sus credenciales no eran válidas. De forma predeterminada, en caso de error el inicio de sesión control simplemente establece su `FailureText` etiqueta de propiedad de texto del control a un mensaje de error (el intento de inicio de sesión no fue correcto. Vuelva a intentarlo). Sin embargo, si el control de inicio de sesión [ `FailureAction` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) está establecido en `RedirectToLoginPage`, a continuación, los problemas de control de inicio de sesión una `Response.Redirect` a la página de inicio de sesión anexar el parámetro querystring `loginfailure=1` (lo que hace que el inicio de sesión control para mostrar el mensaje de error).

En la ilustración 9 se ofrece un diagrama de flujo del flujo de trabajo de autenticación.


[![Flujo de trabajo de autenticación del Control de inicio de sesión](validating-user-credentials-against-the-membership-user-store-vb/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image25.png)

**Figura 9**: flujo de trabajo de autenticación del Control de inicio de sesión ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image27.png))


> [!NOTE]
> Si desea saber cuándo debe utilizar el `FailureAction`de `RedirectToLogin` página opción, considere el escenario siguiente. Ahora nuestro `Site.master` página maestra actualmente tiene el texto, Hello, stranger se muestran en la columna izquierda cuando visita por un usuario anónimo, pero imaginemos que deseamos reemplazar el texto con un control de inicio de sesión. Esto le permitiría a iniciar sesión desde cualquier página del sitio, en lugar de requerir que visite la página de inicio de sesión directamente un usuario anónimo. Sin embargo, si un usuario no pudo iniciar sesión a través del control de inicio de sesión representado por la página maestra, tendría sentido redirigirlas a la página de inicio de sesión (`Login.aspx`) porque esa página es probable que incluye instrucciones adicionales, vínculos así como otro ayuda - como vínculos para crear un nueva cuenta o recuperar una contraseña perdida - que no se han agregado a la página maestra.


### <a name="creating-theauthenticateevent-handler"></a>Crear el`Authenticate`controlador de eventos

Para agregar la lógica de autenticación personalizada, necesitamos crear un controlador de eventos para el control de inicio de sesión `Authenticate` eventos. Crear un controlador de eventos para el `Authenticate` evento generará la siguiente definición de controlador de eventos:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample3.vb)]

Como puede ver, el `Authenticate` controlador de eventos se pasa un objeto de tipo [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) como su segundo parámetro de entrada. El `AuthenticateEventArgs` clase contiene una propiedad booleana denominada `Authenticated` que se utiliza para especificar si las credenciales proporcionadas son válidas. Nuestro tarea, a continuación, consiste en escribir código aquí que determina si las credenciales proporcionadas son válidas o no y para establecer el `e.Authenticate` propiedad según corresponda.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinar y validar las credenciales proporcionadas

Utilice el control de inicio de sesión [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) y [ `Password` propiedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) para determinar las credenciales de nombre de usuario y una contraseña escritas por el usuario. Para determinar los valores especificados en los controles Web adicionales (como el `Email` cuadro de texto se agregó en el paso anterior), utilice `LoginControlID.FindControl`("*`controlID`*") para obtener una referencia a la Web mediante programación control de la plantilla cuya `ID` propiedad es igual a  *`controlID`* . Por ejemplo, para obtener una referencia a la `Email` cuadro de texto, utilice el código siguiente:

`Dim EmailTextBox As TextBox = CType(myLogin.FindControl("Email"), TextBox)`

Para validar las credenciales del usuario que necesitamos hacer dos cosas:

1. Asegúrese de que el nombre de usuario proporcionado y la contraseña son válidos
2. Asegúrese de que dirección de correo electrónico que escribió coincide con la dirección de correo electrónico en el archivo para el usuario intenta iniciar una sesión

Para realizar la primera comprobación que podemos usar simplemente la `Membership.ValidateUser` método como vimos en el paso 1. Para la segunda comprobación, es necesario determinar la dirección de correo electrónico del usuario para que se puede comparar con la dirección de correo electrónico que escribió en el control de cuadro de texto. Para obtener información acerca de un usuario determinado, use la `Membership` la clase [ `GetUser` método](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

El `GetUser` método tiene varias sobrecargas. Si se utiliza sin pasar los parámetros, devuelve información sobre el usuario conectado actualmente. Para obtener información sobre un usuario determinado, llame `GetUser` pasando su nombre de usuario. En cualquier caso, `GetUser` devuelve un [ `MembershipUser` objeto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que tiene propiedades como `UserName`, `Email`, `IsApproved`, `IsOnline`, y así sucesivamente.

El código siguiente implementa estas comprobaciones de dos. Si ambos pasan, a continuación, `e.Authenticate` está establecido en `True`, en caso contrario, se le asigna `False`.

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample4.vb)]

Con este código en su lugar, intente iniciar la sesión como usuario válido, especifique el nombre de usuario correcto, la contraseña y la dirección de correo electrónico. Inténtelo de nuevo, pero esta vez intencionadamente, use una dirección de correo electrónico incorrecto (consulte la figura 10). Por último, pruébelo una tercera vez con un nombre de usuario no existe. En el primer caso debe estar iniciado sesión correctamente en el sitio, pero en los dos últimos casos debería ver el mensaje de credenciales no válidas del control de inicio de sesión.


[![Tito no puede iniciar sesión cuando se suministra una dirección de correo electrónico incorrecto](validating-user-credentials-against-the-membership-user-store-vb/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image28.png)

**Figura 10**: Tito no registro en al proporcionar una dirección de correo electrónico incorrecta ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image30.png))


> [!NOTE]
> Como se describe en la sección cómo la pertenencia Framework controla válido inicios de sesión en el paso 1, cuando la `Membership.ValidateUser` método se llama y se pasan credenciales no válidas, se realiza un seguimiento del intento de inicio de sesión no válido y el usuario se bloqueará si superan un determinado umbral de intentos no válidos dentro de un período de tiempo especificado. Desde nuestras llamadas de lógica de autenticación personalizada el `ValidateUser` método, una contraseña incorrecta para un nombre de usuario válido incrementará el contador de intento de inicio de sesión no válido, pero este contador no se incrementa en el caso donde el nombre de usuario y la contraseña son válidos, pero el dirección de correo electrónico no es correcta. Lo más probable es que este comportamiento es adecuado, ya que es probable que un hacker se conoce el nombre de usuario y la contraseña, pero tiene que usar técnicas de fuerza bruta para determinar la dirección de correo electrónico del usuario.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Paso 4: Mejorar el mensaje de credenciales no válidas del Control de inicio de sesión

Cuando un usuario intenta iniciar sesión con credenciales válidas, el control de inicio de sesión muestra un mensaje que explica que el intento de inicio de sesión no tuvo éxito. En concreto, el control muestra el mensaje especificado por su [ `FailureText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), que tiene un valor predeterminado de su intento de inicio de sesión no fue correcta. Vuelva a intentarlo.

Recuerde que hay muchas razones por qué las credenciales de un usuario pueden ser no válidas:

- El nombre de usuario no exista.
- El nombre de usuario existe pero la contraseña no es válida
- El nombre de usuario y la contraseña son válidos, pero el usuario aún no está aprobado
- El nombre de usuario y la contraseña son válidos, pero el usuario está bloqueado fuera (probablemente porque superó el número de intentos de inicio de sesión no válido en el período de tiempo especificado)

Y puede haber otros motivos cuando se usa la lógica de autenticación personalizada. Por ejemplo, con el código se escribió en el paso 3, el nombre de usuario y contraseña sea válido, pero la dirección de correo electrónico puede ser incorrecta.

Independientemente de por qué las credenciales son válidas, el control de inicio de sesión muestra el mismo mensaje de error. Esta falta de comentarios puede resultar confusa para un usuario cuya cuenta aún no se ha aprobado o que se ha bloqueado. Con un poco de trabajo, sin embargo, podemos tenemos el control de inicio de sesión se muestra un mensaje más apropiado.

Cada vez que un usuario intenta iniciar sesión con credenciales válidas, el control de inicio de sesión provoca su `LoginError` eventos. Siga adelante y crear un controlador de eventos para este evento y agregue el código siguiente:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample5.vb)]

El código anterior se inicia al establecer el control de inicio de sesión `FailureText` propiedad en el valor predeterminado (el intento de inicio de sesión no fue correcto. Vuelva a intentarlo). A continuación, comprueba si el nombre de usuario proporcionado se asigna a una cuenta de usuario existente. Si es así, consulta resultante `MembershipUser` del objeto `IsLockedOut` y `IsApproved` propiedades para determinar si la cuenta se ha bloqueado o que no han sido aprobada. En cualquier caso, el `FailureText` propiedad se actualiza a un valor correspondiente.

Para probar este código, intencionadamente intente iniciar sesión como un usuario existente, pero utiliza una contraseña incorrecta. Este cinco veces en una fila dentro de un período de 10 minutos y se bloqueará la cuenta. Como se muestra en la figura 11, inicio de sesión posterior intentos siempre se producirá un error (incluso con la contraseña correcta), pero ahora para mostrar más descriptivo su cuenta se ha bloqueado debido a demasiados intentos incorrectos de inicio de sesión no válido. Póngase en contacto con el administrador para que el mensaje desbloquear cuenta.


[![Tito realizado demasiados intentos incorrectos de inicio de sesión no válido y se ha bloqueado](validating-user-credentials-against-the-membership-user-store-vb/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image31.png)

**Figura 11**: Tito realiza demasiado muchos intentos no válidos de inicio de sesión y ha sido bloqueada ([haga clic aquí para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-vb/_static/image33.png))


## <a name="summary"></a>Resumen

Anterior en este tutorial, nuestra página de inicio de sesión valida las credenciales proporcionadas con una lista de pares de nombre de usuario/contraseña codificados de forma rígida. En este tutorial, se actualiza la página para validar las credenciales con el marco de trabajo de pertenencia. En el paso 1, observamos utilizando el `Membership.ValidateUser` método mediante programación. En el paso 2 se reemplazan la interfaz de usuario creada manualmente y el código con el control de inicio de sesión.

El control de inicio de sesión representa una interfaz de usuario de inicio de sesión estándar y valida automáticamente las credenciales del usuario en el marco de trabajo de pertenencia. Además, en caso de credenciales válidas, el control de inicio de sesión inicia sesión el usuario en a través de la autenticación de formularios. En resumen, está disponible una experiencia de usuario de inicio de sesión totalmente funcional simplemente arrastrando el control de inicio de sesión en una página, ningún marcado declarativo adicional o el código necesario. ¿Qué es más, el control de inicio de sesión es muy personalizable, lo que permite un gran nivel de control sobre ambos la lógica de autenticación y la interfaz de usuario representado.

En este momento los visitantes de nuestro sitio Web pueden crear una nueva cuenta de usuario y un registro en el sitio, pero aún no hemos mirar restringir el acceso a las páginas según el usuario autenticado. Actualmente, cualquier usuario autenticado o anónimo, puede ver cualquier página en nuestro sitio. Además de controlar el acceso a las páginas de nuestro sitio según un usuario por usuario, tengamos que tenemos algunas páginas cuya funcionalidad depende del usuario. El siguiente tutorial examina cómo limitar el acceso y la funcionalidad de en la página en función de la sesión del usuario.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Mostrar mensajes personalizados que se bloquee y los usuarios no aprobado](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Examen de ASP.NET del 2.0 perfil, Roles y pertenencia](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Cómo: Crear una página de inicio de sesión ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentación técnica de Control de inicio de sesión](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Uso de los controles de inicio de sesión](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial fueron Teresa Murphy y Michael Olivero. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Anterior](creating-user-accounts-vb.md)
[Siguiente](user-based-authorization-vb.md)
