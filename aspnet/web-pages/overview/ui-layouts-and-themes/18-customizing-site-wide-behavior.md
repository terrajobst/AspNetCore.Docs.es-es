---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Personalizar el comportamiento de todo el sitio para ASP.NET Web Pages (Razor) sitios | Microsoft Docs
author: Rick-Anderson
description: Este capítulo explica cómo realizar la configuración en todo el sitio Web o una carpeta completa, en lugar de simplemente una página.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: ca7c241d6e4d1e4baa581faf2bda4ed275b4e785
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021474"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Personalizar el comportamiento de todo el sitio para los sitios de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo hacer que la configuración de sitio para las páginas en un sitio Web de ASP.NET Web Pages (Razor).
> 
> Lo que aprenderá:
> 
> - Cómo ejecutar código que le permita conjunto de valores (valores globales o configuración de aplicación auxiliar) para todas las páginas de un sitio.
> - Cómo ejecutar código que le permite establecer los valores de todas las páginas en una carpeta.
> - Cómo ejecutar código antes y después de una página de carga.
> - Cómo enviar errores a una página de error central.
> - Cómo agregar autenticación a todas las páginas en una carpeta.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library (paquete de NuGet)
>   
> 
> Este tutorial también funciona con 3 páginas Web de ASP.NET y Visual Studio 2013 (o Visual Studio Express 2013 para Web), excepto que no puede usar la biblioteca de aplicaciones auxiliares de Web de ASP.NET.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Agregar código de inicio del sitio Web de ASP.NET Web Pages

Para gran parte del código que se escribe en ASP.NET Web Pages, una página individual puede contener todo el código necesario para dicha página. Por ejemplo, si una página envía un mensaje de correo electrónico, es posible colocar todo el código para esa operación en una sola página. Esto puede incluir el código para inicializar la configuración para enviar correo electrónico (es decir, para el servidor SMTP) y para enviar el mensaje de correo electrónico.

Sin embargo, en algunas situaciones, es posible que desee ejecutar código antes de cualquier página en el sitio se ejecuta. Esto es útil para establecer los valores que se pueden usar en cualquier lugar en el sitio (denominados *valores globales*.) Por ejemplo, algunas aplicaciones auxiliares requieren que proporcione valores como configuración de correo electrónico o las claves de cuenta. Puede ser útil para mantener esta configuración en los valores globales.

Puede hacerlo mediante la creación de una página denominada  *\_AppStart.cshtml* en la raíz del sitio. Si esta página no existe, se ejecuta la primera vez que se solicita cualquier página del sitio. Por lo tanto, es un buen lugar para ejecutar el código para establecer los valores globales. (Porque  *\_AppStart.cshtml* tiene un prefijo de subrayado, ASP.NET no enviará la página en un explorador, incluso si los usuarios solicitar directamente.)

El siguiente diagrama muestra cómo el  *\_AppStart.cshtml* página funciona. Cuando llega una solicitud para una página y, si se trata de la primera solicitud para cualquier página en el sitio, ASP.NET comprueba primero si un  *\_AppStart.cshtml* página existe. Si es así, cualquier código presente en el  *\_AppStart.cshtml* página se ejecuta y, a continuación, se ejecuta la página solicitada.

![[image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Configuración de valores globales para su sitio Web

1. En la carpeta raíz de un sitio Web de WebMatrix, cree un archivo denominado  *\_AppStart.cshtml*. El archivo debe estar en la raíz del sitio.
2. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Este código almacena un valor en el `AppState` diccionario, que está disponible automáticamente para todas las páginas del sitio. Tenga en cuenta que el  *\_AppStart.cshtml* archivo no tiene ningún marcado en ella. La página se ejecute el código y, a continuación, redirigir a la página solicitada originalmente.

    > [!NOTE]
    > Tenga cuidado al colocar código en el  *\_AppStart.cshtml* archivo. Si se produce algún error en el código en el  *\_AppStart.cshtml* archivo, no se iniciará el sitio Web.
3. En la carpeta raíz, cree una nueva página denominada *AppName.cshtml*.
4. Reemplace el marcado predeterminado y el código con lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Este código extrae el valor de la `AppState` objeto configuradas en el  *\_AppStart.cshtml* página.
5. Ejecute el *AppName.cshtml* página en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) La página muestra el valor global. 

    ![[image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Valores de configuración para las aplicaciones auxiliares

Un buen uso de la  *\_AppStart.cshtml* archivo consiste en establecer valores para las aplicaciones auxiliares que se utilice en su sitio y que tienen que inicializarse. Ejemplos típicos son configuraciones de correo electrónico para el `WebMail` auxiliar y las claves públicas y privadas para la `ReCaptcha` auxiliar. En estos casos, puede establecer los valores de una vez en el  *\_AppStart.cshtml* y, a continuación, están ya establecidos para todas las páginas del sitio.

Este procedimiento muestra cómo establecer `WebMail` configuración global. (Para obtener más información sobre el uso de la `WebMail` auxiliares, consulte [agregando un correo electrónico a un sitio de ASP.NET Web Pages](../getting-started/11-adding-email-to-your-web-site.md).)

1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no lo ha agregado.
2. Si aún no tiene un  *\_AppStart.cshtml* , en la carpeta raíz de un sitio Web, cree un archivo denominado  *\_AppStart.cshtml*.
3. Agregue el siguiente `WebMail` configuración para el  *\_AppStart.cshtml* archivo: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Modificar la configuración relacionada en el código de correo electrónico lo siguiente:

   - Establecer `your-SMTP-host` en el nombre del servidor SMTP que tienen acceso a.
   - Establecer `your-user-name-here` al nombre de usuario de la cuenta del servidor SMTP.
   - Establecer `your-account-password` a la contraseña de la cuenta del servidor SMTP.
   - Establecer `your-email-address-here` a su propia dirección de correo electrónico. Se trata de la dirección de correo electrónico que se envía el mensaje desde. (Algunos proveedores de correo electrónico no le permiten especificar otro `From` de direcciones y usará el nombre de usuario como la `From` dirección.)

     Para obtener más información acerca de la configuración de SMTP, vea [configuración de correo electrónico](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) en el artículo [enviar correo electrónico desde un sitio de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) y [problemas con el envío de correo electrónico](https://go.microsoft.com/fwlink/?LinkId=253001#email)en el [de ASP.NET Web Pages (Razor) Guía de solución de problemas](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Guardar el  *\_AppStart.cshtml* de archivo y ciérrelo.
5. En la carpeta raíz de un sitio Web, cree la nueva página denominada *TestEmail.cshtml*.
6. Reemplace el contenido existente por lo siguiente: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Ejecute el *TestEmail.cshtml* página en un explorador.
8. Rellene los campos para enviarse a usted mismo un mensaje de correo electrónico y, a continuación, haga clic en **enviar**.
9. Compruebe su correo electrónico para asegurarse de que ha llegado el mensaje.

La parte importante de este ejemplo es que la configuración que no suele cambiar, como el nombre del servidor SMTP y las credenciales de correo electrónico, se establecen en el  *\_AppStart.cshtml* archivo. De este modo que no es necesario establecerlas de nuevo en cada página donde enviar correo electrónico. (Aunque si por alguna razón necesita cambiar esta configuración, puede establecer ellos individualmente en una página.) En la página, solo hay que establecer los valores que suele cambian cada vez, al igual que el destinatario y el cuerpo del mensaje de correo electrónico.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Ejecutar código antes y después de los archivos en una carpeta

Al igual que puede usar  *\_AppStart.cshtml* para escribir código antes de ejecutan las páginas del sitio, puede escribir código que se ejecuta antes (y después de) cualquier página en una carpeta concreta que se ejecute. Esto es útil para cosas como el establecimiento de la misma página de diseño para todas las páginas en una carpeta, o para comprobar que un usuario ha iniciado sesión antes de ejecutar una página en la carpeta.

Para las páginas en particular las carpetas, puede crear código en un archivo denominado  *\_PageStart.cshtml*. El siguiente diagrama muestra cómo el  *\_PageStart.cshtml* página funciona. Cuando llega una solicitud para una página, ASP.NET comprueba primero un  *\_AppStart.cshtml* página y que ejecuta. A continuación, ASP.NET comprueba si hay un  *\_PageStart.cshtml* página y, si es así, que se ejecuta. A continuación, ejecuta la página solicitada.

Dentro de la  *\_PageStart.cshtml* página, puede especificar dónde durante el procesamiento que desee en la página solicitada para ejecutar mediante la inclusión de un `RunPage` método. Esto le permite ejecutar código antes de que se ejecuta la página solicitada y, a continuación, otra vez después de él. Si no incluye `RunPage`, todo el código de  *\_PageStart.cshtml* ejecuciones y, a continuación, en la página solicitada se ejecuta automáticamente.

![[image]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET permite crear una jerarquía de  *\_PageStart.cshtml* archivos. Puede colocar un  *\_PageStart.cshtml* archivo en la raíz del sitio y en cualquier subcarpeta. Cuando se solicita una página, el  *\_PageStart.cshtml* archivo en las ejecuciones de nivel superior (más cercana a la raíz del sitio), seguido por el  *\_PageStart.cshtml* archivo en el siguiente subcarpeta, y así sucesivamente hacia abajo de la estructura de subcarpetas hasta que la solicitud llega a la carpeta que contiene la página solicitada. Después de todos los aplicable  *\_PageStart.cshtml* han ejecutado archivos, se ejecuta la página solicitada.

Por ejemplo, podría tener la siguiente combinación de  *\_PageStart.cshtml* archivos y *Default.cshtml* archivo:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Al ejecutar */myfolder/default.cshtml*, verá lo siguiente:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Ejecutar código de inicialización para todas las páginas en una carpeta

Un buen uso de  *\_PageStart.cshtml* archivos consiste en inicializar la misma página de diseño para todos los archivos en una única carpeta.

1. En la carpeta raíz, cree una carpeta nueva denominada *InitPages*.
2. En el *InitPages* carpeta del sitio Web, cree un archivo denominado  *\_PageStart.cshtml* y reemplácelo por el marcado predeterminado y el código siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. En la raíz del sitio Web, cree una carpeta denominada *Shared*.
4. En el *Shared* carpeta, cree un archivo denominado  *\_Layout1.cshtml* y reemplácelo por el marcado predeterminado y el código siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. En el *InitPages* carpeta, cree un archivo denominado *Content1.cshtml* y reemplace el contenido existente por lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. En el *InitPages* carpeta, cree otro archivo llamado *Content2.cshtml* y reemplace el marcado predeterminado con lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Ejecute *Content1.cshtml* en un explorador. 

    ![[image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Cuando el *Content1.cshtml* página se ejecuta, el  *\_PageStart.cshtml* archivo establece `Layout` y también establece `PageData["MyBackground"]` a un color. En *Content1.cshtml*, el diseño y el color se aplican.
8. Mostrar *Content2.cshtml* en un explorador. 

    El diseño es el mismo, porque ambas páginas utilizan la misma página de diseño y el color como inicializado en  *\_PageStart.cshtml*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Uso de \_PageStart.cshtml para controlar los errores

Usa otra buena para el  *\_PageStart.cshtml* es crear un medio para controlar los errores de programación (excepciones) que pueden producirse en cualquier archivo *.cshtml* página en una carpeta. En este ejemplo se muestra una manera de hacerlo.

1. En la carpeta raíz, cree una carpeta denominada *InitCatch*.
2. En el *InitCatch* carpeta del sitio Web, cree un archivo denominado  *\_PageStart.cshtml* y reemplácelo por el marcado existente y el código siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    En este código, intente ejecutar la página solicitada explícitamente mediante una llamada a la `RunPage` método dentro de un `try` bloque. Si se produce algún error de programación en la solicitud de página, el código dentro de la `catch` bloquear ejecuciones. En este caso, el código redirige a una página (*Error.cshtml*) y pasa el nombre del archivo que encontró el error como parte de la dirección URL. (Se creará la página en breve.)
3. En el *InitCatch* carpeta del sitio Web, cree un archivo denominado *Exception.cshtml* y reemplácelo por el marcado existente y el código siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Para fines de este ejemplo, lo que está haciendo en esta página es deliberadamente creando un error al intentar abrir un archivo de base de datos que no existe.
4. En la carpeta raíz, cree un archivo denominado *Error.cshtml* y reemplácelo por el marcado existente y el código siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    En esta página, la expresión `@Request["source"]` Obtiene el valor de la dirección URL y lo muestra.
5. En la barra de herramientas, haga clic en **guardar**.
6. Ejecute *Exception.cshtml* en un explorador. 

    ![[image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Se produce un error en *Exception.cshtml*, el  *\_PageStart.cshtml* página redirige a la *Error.cshtml* archivo, que muestra el mensaje.

    Para obtener más información sobre las excepciones, vea [Introducción a ASP.NET Web Pages de programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Uso de \_PageStart.cshtml para restringir el acceso de carpeta

También puede usar el  *\_PageStart.cshtml* archivos para restringir el acceso a todos los archivos en una carpeta.

1. En WebMatrix, cree un nuevo sitio Web mediante la **sitio desde plantilla** opción.
2. En las plantillas disponibles, seleccione **Starter Site**.
3. En la carpeta raíz, cree una carpeta denominada *AuthenticatedContent*.
4. En el *AuthenticatedContent* carpeta, cree un archivo denominado  *\_PageStart.cshtml* y reemplácelo por el marcado existente y el código siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    El código se inicia al impedir que todos los archivos en la carpeta que se va a almacenar en caché. (Esto es necesario para escenarios como los equipos públicos, si no desea páginas en caché de un usuario esté disponible para el usuario siguiente). A continuación, el código determina si el usuario ha iniciado sesión en el sitio antes de poder ver cualquiera de las páginas en la carpeta. Si el usuario no ha iniciado sesión, el código redirige a la página de inicio de sesión. La página de inicio de sesión puede devolver al usuario a la página solicitada originalmente si incluye un valor de cadena de consulta denominado `ReturnUrl`.
5. Crear una nueva página en el *AuthenticatedContent* carpeta denominada *Page.cshtml*.
6. Reemplace el marcado predeterminado con lo siguiente:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Ejecute *Page.cshtml* en un explorador. El código le redirige a una página de inicio de sesión. Debe registrar antes de iniciar sesión. Después de que ha registrado y ha iniciado sesión, puede navegar a la página y ver su contenido.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Introducción a ASP.NET Web Pages usando la sintaxis Razor de programación](https://go.microsoft.com/fwlink/?LinkID=251587)
