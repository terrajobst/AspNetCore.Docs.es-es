---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: "Enviar correo electrónico desde un sitio Web de ASP.NET Pages (Razor) sitio | Documentos de Microsoft"
author: tfitzmac
description: "Este capítulo explica cómo enviar un mensaje de correo electrónico automatizadas desde un sitio Web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: c5878c3bc468daef050dcebee99f64441066409a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Enviar correo electrónico de un sitio de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo enviar un mensaje de correo electrónico desde un sitio Web cuando se usa ASP.NET Web Pages (Razor).
> 
> Lo que aprenderá:
> 
> - Cómo enviar un mensaje de correo electrónico de su sitio Web.
> - Cómo adjuntar un archivo a un mensaje de correo electrónico.
> 
> Se trata de la característica ASP.NET que se introdujo en el artículo:
> 
> - El `WebMail` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Enviar mensajes de correo electrónico de su sitio Web

Hay todo tipo de razones por las que podría necesitar para enviar correo electrónico desde su sitio Web. Puede enviar mensajes de confirmación a los usuarios, o puede enviar notificaciones a sí mismo (por ejemplo, que se ha registrado un usuario nuevo.) El `WebMail` auxiliar facilita el proceso para enviar correo electrónico.

Para usar el `WebMail` auxiliar, tiene que tener acceso a un servidor SMTP. (Es el acrónimo SMTP *Protocolo Simple de transferencia de correo electrónico*.) Un servidor SMTP es un servidor de correo electrónico que sólo reenvía mensajes para el destinatario servidor &#8212; es la parte de la salida de correo electrónico. Si usa un proveedor de hospedaje de su sitio Web, probablemente configurarle correo electrónico y puede saber cuál puede ser el nombre del servidor SMTP. Si está trabajando dentro de una red corporativa, un administrador o el departamento de TI normalmente puede proporcionar la información sobre un servidor SMTP que puede usar. Si trabaja en casa, incluso puede probar mediante el proveedor de correo electrónico normal, que puede indicar el nombre de su servidor SMTP. Normalmente debe:

- El nombre del servidor SMTP.
- El número de puerto. Esto casi siempre es 25. Sin embargo, puede que el ISP requiera que use el puerto 587. Si está utilizando capa de sockets seguros (SSL) para correo electrónico, tendrá que un puerto diferente. Póngase en contacto con su proveedor de correo electrónico.
- Credenciales (nombre de usuario, una contraseña).

En este procedimiento, creará dos páginas. La primera página tiene un formulario que permite a los usuarios escribir una descripción, como si se rellena de un formulario de soporte técnico. La primera página envía su información a una segunda página. En la segunda página, el código extrae la información del usuario y envía un mensaje de correo electrónico. También muestra un mensaje que confirma que se ha recibido el informe de problemas.

![[image]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Para simplificar este ejemplo, el código inicializa el `WebMail` derecha de la aplicación auxiliar en la página donde se utilice. Sin embargo, para los sitios Web real, es una idea más clara que coloque el código de inicialización similar al siguiente en un archivo global, por lo que inicialice la `WebMail` auxiliar para todos los archivos en su sitio Web. Para obtener más información, consulte [personalizar el comportamiento de todo el sitio para ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Crear un nuevo sitio Web.
2. Agregar una nueva página denominada *EmailRequest.cshtml* y agregue el siguiente marcado: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Tenga en cuenta que la `action` se ha establecido el atributo del elemento de formulario en *ProcessRequest.cshtml*. Esto significa que el formulario se enviarán a esa página en lugar de volver a la página actual.
3. Agregar una nueva página denominada *ProcessRequest.cshtml* al sitio Web y agregue el código y el marcado siguiente:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    En el código, obtener los valores de los campos de formulario que se enviaron a la página. A continuación, llame a la `WebMail` del auxiliar `Send` método para crear y enviar el mensaje de correo electrónico. En este caso, los valores se componen de texto que concatena con los valores que se enviaron desde el formulario.

    El código de esta página está dentro de un `try/catch` bloque. Si por cualquier motivo, al intentar enviar un correo electrónico no funciona (por ejemplo, la configuración no es correcta), el código en el `catch` bloque se ejecuta y establece el `errorMessage` variable para el error que se ha producido. (Para obtener más información acerca de `try/catch` bloques o `<text>` de etiquetas, consulte [Introducción a ASP.NET Web Pages de programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    En el cuerpo de la página, si la `errorMessage` variable está vacía (el valor predeterminado), el usuario ve un mensaje que se ha enviado el mensaje de correo electrónico. Si el `errorMessage` variable se establece en true, el usuario ve un mensaje que ha habido un problema al enviar el mensaje.

    Observe que en la parte de la página que muestra un mensaje de error, es una prueba adicional: `if(debuggingFlag)`. Se trata de una variable que se puede establecer en true si tiene problemas para enviar correo electrónico. Cuando `debuggingFlag` es true, y si hay un problema al enviar correo electrónico, se muestra un mensaje de error adicional que muestra lo que ASP.NET ha notificado al intentar enviar el mensaje de correo electrónico. Advertencia razonable, aunque: los mensajes de error que informa de ASP.NET cuando no puede enviar un mensaje de correo electrónico pueden ser genéricos. Por ejemplo, si ASP.NET no puede contactar con el servidor SMTP (por ejemplo, porque ha cometido un error en el nombre del servidor), el error es `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Importante** al obtener un mensaje de error de un objeto de excepción (`ex` en el código), realice *no* habitualmente pasar ese mensaje a través de a los usuarios. A menudo, los objetos de excepción incluyen información que los usuarios no deben ver y que incluso puede ser una vulnerabilidad de seguridad. Por eso este código incluye la variable `debuggingFlag` que se utiliza como modificador para mostrar el mensaje de error, y por qué la variable de forma predeterminada se establece en false. Debe configurar esa variable en true (y, por tanto, mostrar el mensaje de error) *sólo* si experimenta un problema con el envío de correo electrónico y tiene que depurar. Una vez resueltos los problemas, establecer `debuggingFlag` a false.

    Modificar la configuración relacionada en el código de correo electrónico de los siguientes:

    - Establecer `your-SMTP-host` en el nombre del servidor SMTP que tienen acceso a.
    - Establecer `your-user-name-here` al nombre de usuario para la cuenta del servidor SMTP.
    - Establecer `your-account-password` como la contraseña para la cuenta del servidor SMTP.
    - Establecer `your-email-address-here` a su propia dirección de correo electrónico. Se trata de la dirección de correo electrónico que se envía el mensaje desde. (Algunos proveedores de correo electrónico no permiten especificar otro `From` de direcciones y usará el nombre de usuario como la `From` dirección.)

    > [!TIP] 
    > 
    > <a id="configuring_email_settings"></a>
    > ### <a name="configuring-email-settings"></a>Configuración de correo electrónico
    > 
    > Puede ser un desafío en ocasiones para asegurarse de que tiene la configuración de derechos para el servidor SMTP, número de puerto y así sucesivamente. A continuación se muestran algunas sugerencias:
    > 
    > - El nombre del servidor SMTP suele ser algo parecido a `smtp.provider.com` o `smtp.provider.net`. Sin embargo, si se publica un sitio en un proveedor de hospedaje, el nombre del servidor SMTP en ese momento podría ser `localhost`. Esto es porque una vez que ha publicado y el sitio se ejecuta en el servidor del proveedor, el servidor de correo electrónico puede ser local desde la perspectiva de la aplicación. Este cambio en los nombres de servidor, podría significar que tiene que cambiar el nombre del servidor SMTP como parte del proceso de publicación.
    > - Normalmente, el número de puerto es 25. Sin embargo, algunos proveedores requieren que se va a utilizar el puerto 587 o algún otro puerto.
    > - Asegúrese de que utiliza las credenciales correctas. Si ha publicado su sitio en un proveedor de hospedaje, utilice las credenciales que el proveedor ha indicado específicamente son para correo electrónico. Estos podrían ser diferentes de las credenciales que use para publicar.
    > - En ocasiones, no necesita credenciales en absoluto. Si va a enviar correo electrónico mediante el ISP personal, el proveedor de correo electrónico ya quizá sepa que sus credenciales. Después de publicar, debe utilizar credenciales diferentes a cuando se prueba en el equipo local.
    > - Si su proveedor de correo electrónico usa cifrado, tendrá que configurar `WebMail.EnableSsl` a `true`.
4. Ejecute el *EmailRequest.cshtml* página en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)
5. Escriba su nombre y una descripción del problema y, a continuación, haga clic en el **enviar** botón. Se le redirigirá a la *ProcessRequest.cshtml* página, que confirma el mensaje y que envía un mensaje de correo electrónico. 

    ![[image]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Envío de un archivo mediante correo electrónico

También puede enviar archivos que están conectados a los mensajes de correo electrónico. En este procedimiento, creará un archivo de texto y dos páginas HTML. Deberá usar el archivo de texto como datos adjuntos de correo electrónico.

1. En el sitio Web, agregue un nuevo archivo de texto y asígnele el nombre *MyFile.txt*.
2. Copie el texto siguiente y péguelo en el archivo: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Crear una página denominada *SendFile.cshtml* y agregue el siguiente marcado: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Crear una página denominada *ProcessFile.cshtml* y agregue el siguiente marcado: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modificar la configuración relacionada en el código del ejemplo de correo electrónico de los siguientes:

    - Establecer `your-SMTP-host` en el nombre de un servidor SMTP que tiene acceso a.
    - Establecer `your-user-name-here` al nombre de usuario para la cuenta del servidor SMTP.
    - Establecer `your-email-address-here` a su propia dirección de correo electrónico. Se trata de la dirección de correo electrónico que se envía el mensaje desde.
    - Establecer `your-account-password` como la contraseña para la cuenta del servidor SMTP.
    - Establecer `target-email-address-here` a su propia dirección de correo electrónico. (Como antes, normalmente enviaría un correo electrónico a otra persona, pero para las pruebas, puede enviarlo a sí mismo.)
6. Ejecute el *SendFile.cshtml* página en un explorador.
7. Escriba su nombre, una línea de asunto y el nombre del archivo de texto para asociar (*MyFile.txt*).
8. Haga clic en el botón `Submit`. Como antes, se le redirigirá a la *ProcessFile.cshtml* página, que confirma el mensaje y que envía un mensaje de correo electrónico con el archivo adjunto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


- [Guía de solución de problemas de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Protocolo de transferencia de correo simple](https://msdn.microsoft.com/library/aa480435.aspx)
- [Personalizar el comportamiento de todo el sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
