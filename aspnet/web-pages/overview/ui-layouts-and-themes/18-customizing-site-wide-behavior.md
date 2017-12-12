---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Personalizar el comportamiento de todo el sitio para ASP.NET Web Pages (Razor) sitios | Documentos de Microsoft
author: tfitzmac
description: "Este capítulo explica cómo realizar la configuración en todo el sitio o una carpeta completa, en lugar de simplemente una página."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: b1caa26a23517bd976addfefac89375ae965eb91
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Personalizar el comportamiento de todo el sitio para los sitios de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo realizar la configuración de sitio de cliente para las páginas en un sitio Web de ASP.NET Web Pages (Razor).
> 
> Lo que aprenderá:
> 
> - Cómo ejecutar código que le permite conjunto de valores (valores globales o configuración de la aplicación auxiliar) para todas las páginas en un sitio.
> - Cómo ejecutar código que permite establecer valores para todas las páginas en una carpeta.
> - Cómo ejecutar código antes y después de una página de carga.
> - Cómo enviar errores a una página de error central.
> - Cómo agregar autenticación a todas las páginas en una carpeta.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - 3 de WebMatrix
> - ASP.NET Web Helpers Library (paquete de NuGet)
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 3 y no puede usar Visual Studio 2013 (o Visual Studio Express 2013 para Web), excepto si ASP.NET Web Helpers Library.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Agregar código de inicio del sitio Web de ASP.NET Web Pages

Durante gran parte del código que se escribe en las páginas Web ASP.NET, una página individual puede contener todo el código que se necesita para esa página. Por ejemplo, si una página envía un mensaje de correo electrónico, es posible colocar todo el código para esa operación en una sola página. Esto puede incluir el código para inicializar los valores para el envío de correo electrónico (es decir, para el servidor SMTP) y para enviar el mensaje de correo electrónico.

Sin embargo, en algunas situaciones, puede ejecutar el código antes de que se ejecuta cualquier página en el sitio. Esto es útil para establecer los valores que pueden usarse en cualquier lugar en el sitio (denominados *valores globales*.) Por ejemplo, algunas aplicaciones auxiliares de requieran que proporcione valores como valores de configuración de correo electrónico o las claves de cuenta. Puede resultar útil para mantener esta configuración en valores globales.

Puede hacerlo mediante la creación de una página denominada  *\_AppStart.cshtml* en la raíz del sitio. Si no existe esta página, se ejecuta la primera vez que se solicita cualquier página del sitio. Por lo tanto, es un buen lugar para ejecutar el código para establecer los valores globales. (Dado que  *\_AppStart.cshtml* tiene un prefijo de subrayado, ASP.NET no enviará la página en un explorador, incluso si los usuarios solicitar directamente.)

El siguiente diagrama muestra cómo el  *\_AppStart.cshtml* página funciona. Cuando llega una solicitud para una página, y si se trata de la primera solicitud para cualquier página en el sitio, ASP.NET comprueba primero si un  *\_AppStart.cshtml* página existe. Si es así, cualquier código en el  *\_AppStart.cshtml* página se ejecuta y, a continuación, se ejecuta la página solicitada.

![[image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Configurar valores globales para el sitio Web

1. En la carpeta raíz de un sitio Web de WebMatrix, cree un archivo denominado  *\_AppStart.cshtml*. El archivo debe estar en la raíz del sitio.
2. Reemplace el contenido existente con lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Este código almacena un valor en el `AppState` diccionario, lo que está disponible automáticamente para todas las páginas en el sitio. Tenga en cuenta que la  *\_AppStart.cshtml* archivo no tiene todas las marcas en ella. La página se ejecute el código y, a continuación, redirija a la página que se solicitó originalmente.

    > [!NOTE]
    > Tenga cuidado al colocar código en el  *\_AppStart.cshtml* archivo. Si se produce algún error en el código en el  *\_AppStart.cshtml* archivo, no se inicia el sitio Web.
3. En la carpeta raíz, cree una nueva página denominada *AppName.cshtml*.
4. Reemplace el código predeterminado y el código con lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Este código extrae el valor de la `AppState` objeto que ha establecido en el  *\_AppStart.cshtml* página.
5. Ejecute el *AppName.cshtml* página en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) La página muestra el valor global. 

    ![[image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Especifique los valores de las aplicaciones auxiliares

Un buen uso de la  *\_AppStart.cshtml* archivo consiste en definir valores para las aplicaciones auxiliares que utilizar en su sitio y que tienen que inicializarse. Ejemplos típicos son la configuración de correo electrónico para la `WebMail` auxiliar y las claves públicas y privadas para el `ReCaptcha` auxiliar. En estos casos, puede establecer los valores de una vez en el  *\_AppStart.cshtml* y, a continuación, está ya configurados para todas las páginas de su sitio.

Este procedimiento muestra cómo establecer `WebMail` configuración global. (Para obtener más información sobre el uso de la `WebMail` auxiliar, vea [Agregar correo electrónico a un sitio de ASP.NET Web Pages](../getting-started/11-adding-email-to-your-web-site.md).)

1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no se ha agregado.
2. Si aún no tiene un  *\_AppStart.cshtml* , en la carpeta raíz de un sitio Web, cree un archivo denominado  *\_AppStart.cshtml*.
3. Agregue el siguiente `WebMail` configuración para el  *\_AppStart.cshtml* archivo: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Modificar la configuración relacionada en el código de correo electrónico de los siguientes:

    - Establecer `your-SMTP-host` en el nombre del servidor SMTP que tienen acceso a.
    - Establecer `your-user-name-here` al nombre de usuario para la cuenta del servidor SMTP.
    - Establecer `your-account-password` como la contraseña para la cuenta del servidor SMTP.
    - Establecer `your-email-address-here` a su propia dirección de correo electrónico. Se trata de la dirección de correo electrónico que se envía el mensaje desde. (Algunos proveedores de correo electrónico no permiten especificar otro `From` de direcciones y usará el nombre de usuario como la `From` dirección.)

    Para obtener más información acerca de la configuración de SMTP, consulte [configurar opciones de correo electrónico](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) en el artículo [enviar correo electrónico desde un sitio de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) y [problemas con el envío de correo electrónico](https://go.microsoft.com/fwlink/?LinkId=253001#email)en la [de ASP.NET Web Pages (Razor) Guía de solución de problemas de](https://go.microsoft.com/fwlink/?LinkId=253001).
- Guardar el  *\_AppStart.cshtml* archivo y ciérrelo.
- En la carpeta raíz de un sitio Web, cree la nueva página denominada *TestEmail.cshtml*.
- Reemplace el contenido existente con lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
- Ejecute el *TestEmail.cshtml* página en un explorador.
- Rellene los campos para enviarse a sí mismo un mensaje de correo electrónico y, a continuación, haga clic en **enviar**.
- Compruebe su correo electrónico para asegurarse de que ha llegado el mensaje.

La parte importante de este ejemplo es que los valores que normalmente no cambian, le gusta el nombre del servidor SMTP y las credenciales de correo electrónico, se establecen en los  *\_AppStart.cshtml* archivo. De este modo que no es necesario establecer nuevo en cada página donde enviar correo electrónico. (Aunque si por algún motivo necesita cambiar esta configuración, puede establecerlas individualmente en una página.) En la página, solo hay que establecer los valores que suele cambian cada vez, al igual que el destinatario y el cuerpo del mensaje de correo electrónico.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Ejecutar código antes y después de archivos en una carpeta

Al igual que puede usar  *\_AppStart.cshtml* para escribir código antes de que se ejecutan las páginas en el sitio, puede escribir código que se ejecuta antes (y después de) cualquier página de una carpeta concreta que se ejecute. Esto es útil para cosas como el establecimiento de la misma página de diseño para todas las páginas en una carpeta, o para comprobar que un usuario ha iniciado sesión antes de ejecutar una página en la carpeta.

Para las páginas en particular de las carpetas, puede crear código en un archivo denominado  *\_PageStart.cshtml*. El siguiente diagrama muestra cómo el  *\_PageStart.cshtml* página funciona. Cuando llega una solicitud para una página, ASP.NET comprueba primero un  *\_AppStart.cshtml* página y que ejecuta. A continuación, ASP.NET comprueba si hay un  *\_PageStart.cshtml* página y, si es así, que se ejecuta. A continuación, se ejecuta la página solicitada.

Dentro de la  *\_PageStart.cshtml* página, puede especificar dónde durante el procesamiento que desea que la página solicitada para ejecutar mediante la inclusión de un `RunPage` método. Esto le permite ejecutar código antes de que se ejecuta la página solicitada y, a continuación, nuevo después de él. Si no incluye `RunPage`, todo el código de  *\_PageStart.cshtml* se ejecuta y, a continuación, en la página solicitada se ejecuta automáticamente.

![[image]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET le permite crear una jerarquía de  *\_PageStart.cshtml* archivos. Puede colocar una  *\_PageStart.cshtml* archivo en la raíz del sitio y las subcarpetas. Cuando se solicita una página, el  *\_PageStart.cshtml* archivo en las ejecuciones de nivel superior (más cercano a la raíz del sitio), seguido por el  *\_PageStart.cshtml* archivo en el siguiente subcarpeta, y así sucesivamente hacia abajo de la estructura de la subcarpeta hasta que la solicitud llega a la carpeta que contiene la página solicitada. Después de todo el aplicable  *\_PageStart.cshtml* han ejecutado archivos, se ejecuta la página solicitada.

Por ejemplo, podría tener la siguiente combinación de  *\_PageStart.cshtml* archivos y *Default.cshtml* archivo:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Al ejecutar */myfolder/default.cshtml*, verá lo siguiente:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Ejecutar código de inicialización para todas las páginas en una carpeta

Un buen uso de  *\_PageStart.cshtml* archivos consiste en inicializar la misma página de diseño para todos los archivos en una única carpeta.

1. En la carpeta raíz, cree una carpeta nueva denominada *InitPages*.
2. En el *InitPages* carpeta del sitio Web, cree un archivo denominado  *\_PageStart.cshtml* y reemplace el código predeterminado y el código con lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. En la raíz del sitio Web, cree una carpeta denominada *Shared*.
4. En el *Shared* carpeta, cree un archivo denominado  *\_Layout1.cshtml* y reemplace el código predeterminado y el código con lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. En el *InitPages* carpeta, cree un archivo denominado *Content1.cshtml* y reemplace el contenido existente con lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. En el *InitPages* carpeta, cree otro archivo denominado *Content2.cshtml* y reemplace el código predeterminado con lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Ejecutar *Content1.cshtml* en un explorador. 

    ![[image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Cuando el *Content1.cshtml* página se ejecuta, el  *\_PageStart.cshtml* archivo establece `Layout` y también establece `PageData["MyBackground"]` a un color. En *Content1.cshtml*, se aplican el diseño y el color.
8. Mostrar *Content2.cshtml* en un explorador. 

    El diseño es el mismo, ya que ambas páginas usan la misma página de diseño y el color cuando inicializa en  *\_PageStart.cshtml*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Usar \_PageStart.cshtml para controlar los errores

Otra buena usa para la  *\_PageStart.cshtml* archivo consiste en crear un medio para controlar errores de programación (excepciones) que pueden producirse en cualquier *.cshtml* página en una carpeta. En este ejemplo se muestra una manera de hacerlo.

1. En la carpeta raíz, cree una carpeta denominada *InitCatch*.
2. En el *InitCatch* carpeta del sitio Web, cree un archivo denominado  *\_PageStart.cshtml* y reemplace el código y marcado existente con lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    En este código, intente ejecutar la página solicitada explícitamente mediante una llamada a la `RunPage` método dentro de un `try` bloque. Si se produce algún error de programación en la solicitud de página, el código dentro de la `catch` bloquear ejecuciones. En este caso, el código se redirige a una página (*Error.cshtml*) y pasa el nombre del archivo que se ha producido el error como parte de la dirección URL. (Creará la página en breve.)
3. En el *InitCatch* carpeta del sitio Web, cree un archivo denominado *Exception.cshtml* y reemplace el código y marcado existente con lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Para fines de este ejemplo, lo que está haciendo en esta página es deliberadamente creando un error al intentar abrir un archivo de base de datos que no existe.
4. En la carpeta raíz, cree un archivo denominado *Error.cshtml* y reemplace el código y marcado existente con lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    En esta página, la expresión `@Request["source"]` Obtiene el valor de la dirección URL y lo muestra.
5. En la barra de herramientas, haga clic en **guardar**.
6. Ejecutar *Exception.cshtml* en un explorador. 

    ![[image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Se produce un error en *Exception.cshtml*, el  *\_PageStart.cshtml* página redirige a la *Error.cshtml* archivo, que muestra el mensaje.

    Para obtener más información sobre las excepciones, vea [Introducción a ASP.NET Web Pages de programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Usar \_PageStart.cshtml para restringir el acceso a la carpeta

También puede usar el  *\_PageStart.cshtml* archivo para restringir el acceso a todos los archivos en una carpeta.

1. En WebMatrix, cree un nuevo sitio Web mediante la **plantilla de sitio** opción.
2. En las plantillas disponibles, seleccione **Starter Site**.
3. En la carpeta raíz, cree una carpeta denominada *AuthenticatedContent*.
4. En el *AuthenticatedContent* carpeta, cree un archivo denominado  *\_PageStart.cshtml* y reemplace el código y marcado existente con lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    El código se inicia al impedir que se está almacenando en caché todos los archivos en la carpeta. (Esto es necesario para escenarios como los equipos públicos, donde no desea que las páginas en caché de un usuario que estén disponibles para el siguiente usuario). A continuación, el código determina si el usuario ha iniciado sesión en el sitio antes de poder ver cualquiera de las páginas en la carpeta. Si el usuario no ha iniciado sesión, el código se redirige a la página de inicio de sesión. La página de inicio de sesión puede devolver al usuario a la página que se solicitó originalmente si incluye un valor de cadena de consulta denominado `ReturnUrl`.
5. Crear una nueva página en el *AuthenticatedContent* carpeta denominada *Page.cshtml*.
6. Reemplace el código predeterminado con lo siguiente:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Ejecutar *Page.cshtml* en un explorador. El código le redirige a una página de inicio de sesión. Debe registrar antes de iniciar sesión. Una vez que haya registrado e iniciado sesión, puede navegar a la página y ver su contenido.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Introducción a ASP.NET Web Pages programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
