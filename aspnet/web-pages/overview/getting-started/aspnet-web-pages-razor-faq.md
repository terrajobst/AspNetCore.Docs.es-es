---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: "ASP.NET Web Pages (Razor) preguntas más frecuentes | Documentos de Microsoft"
author: tfitzmac
description: "Este artículo enumeran algunas preguntas frecuentes sobre ASP.NET Web Pages (Razor) y WebMatrix. Versiones de software que se usa en el tutorial ASP.NET Web Pages (R..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 7f6dc3b56a33bcbe3e1e4086681ca1ba76d7d153
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) preguntas más frecuentes
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix ya no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) o [código de Visual Studio](https://code.visualstudio.com/).
>
> Este artículo enumeran algunas preguntas frecuentes sobre ASP.NET Web Pages (Razor) y WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - 3 de WebMatrix
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2, 2 de WebMatrix y Visual Studio 2012.


- [¿Cuál es la diferencia entre las páginas Web ASP.NET, ASP.NET Web Forms y ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [¿Es necesario WebMatrix para trabajar con páginas Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [¿Puedo usar controles de formularios Web Forms de ASP.NET en una página de páginas Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [¿Se puede implementar un sitio de ASP.NET Web Pages sin usar WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [¿Tengo que usar la aplicación auxiliar WebSecurity para admitir los inicios de sesión?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [¿Admite ASP.NET Web Pages HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [¿Puedo usar JavaScript y jQuery con páginas Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Recursos adicionales](#AdditionalResources)

Si tiene preguntas sobre los errores y otros problemas, consulte el [Guía de solución de problemas de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>¿Cuál es la diferencia entre las páginas Web ASP.NET, ASP.NET Web Forms y ASP.NET MVC?

Las tres son las tecnologías ASP.NET para crear aplicaciones web dinámicas:

- Las páginas Web ASP.NET se centra en Agregar código dinámico (servidor) y acceso de base de datos a páginas HTML y la sintaxis simple y ligera de características.
- Formularios Web Forms ASP.NET se basa en un modelo de objetos de página y los controles de tipo de ventana tradicional (botones, listas, etcetera). Formularios Web Forms utiliza un modelo basado en eventos que resulte familiar a los que ha trabajado con el desarrollo basado en cliente de (formularios Windows forms).
- ASP.NET MVC implementa el modelo de model-view-controller de ASP.NET. El énfasis se encuentra en "separación de aspectos" (procesamiento, datos y las capas de interfaz de usuario).

Todos los marcos de tres son totalmente compatibles y continuarán se desarrollado por el equipo ASP.NET. En general, depende de la elección de qué framework se utilizará en el fondo y experimentar con ASP.NET.

ASP.NET Web Pages en particular se diseñó para facilitar a las personas que ya conocen HTML para agregar el procesamiento de servidor a sus páginas. Es una buena elección para estudiantes, aficionados, las personas que por lo general que está familiarizado con la programación. También puede ser una buena elección para los desarrolladores que tienen experiencia con las tecnologías web no sean ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>¿Es necesario WebMatrix para trabajar con páginas Web?

No. WebMatrix ya no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) o [código de Visual Studio](https://code.visualstudio.com/).

Si no desea usar Visual Studio o código de Visual Studio, puede instalar los productos de componente individualmente con [instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx). Necesita los siguientes productos:

- Microsoft .NET Framework 4.5
- MVC de ASP.NET 5 (que se instala también el marco de ASP.NET Web Pages)
- IIS Express (el servidor web)
- Microsoft SQL Server Compact 4.0 (la base de datos)

Puede utilizar un editor de texto para editar *.cshtml* (o *.vbhtml*) páginas.

Administrar bases de datos de SQL Server Compact (*.sdf* archivos) sin una herramienta es un poco más difícil. Visual Studio containds tools para administrar *.sdf* bases de datos. También puede ejecutar comandos SQL en el código para realizar muchas tareas de administración de SQL Server.

Para probar *.cshtml* páginas sin usar un entorno de desarrollo integrado (IDE), se pueden implementar en un servidor activo. (Consulte [¿se puede implementar un sitio de ASP.NET Web Pages sin usar WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Ejecutando IIS Express sin usar un IDE

Si instala IIS Express en el equipo como un servidor web, también puede usarlo para probar las páginas. Puede ejecutar IIS Express desde la línea de comandos y asociarlo con un número de puerto específico. A continuación, especificar ese puerto al solicitar *.cshtml* archivos en el explorador.

En Windows, abra un símbolo del sistema con privilegios de administrador y cambie a *C:\Program Files\IIS Express.* (Para sistemas de 64 bits, use la carpeta *C:\Program Files (x86) \IIS Express.)* A continuación, escriba el comando siguiente, con la ruta de acceso real a su sitio:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Puede usar cualquier número de puerto que no esté reservado por otro proceso. (Los números de puerto por encima de 1024 son normalmente libres). Para el `path` valor, use la ruta de acceso de la carpeta del sitio Web donde los *.cshtml* son archivos.

Después de ejecutar este comando para configurar IIS Express para atender las páginas, puede abrir un explorador y vaya a un *.cshtml* archivo. Use una dirección URL similar al siguiente:

`http://localhost:35896/default.cshtml`

Para obtener ayuda con las opciones de línea de comandos de IIS Express, escriba `iisexpress.exe /?` en la línea de comandos.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>¿Puedo usar controles de formularios Web Forms de ASP.NET en una página de páginas Web?

No. Controles de formularios Web como la [casilla](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.checkbox) (control), el [controles de validación](https://msdn.microsoft.com/en-us/library/bwd43d0x)y el [GridView](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview) control sólo funciona en las páginas de formularios Web Forms (*.aspx* archivos). Estos controles requieren que el marco de páginas de formularios Web Forms.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>¿Se puede implementar un sitio de ASP.NET Web Pages sin usar WebMatrix?

Sí. Puede copiar manualmente los archivos del sitio Web en un servidor (normalmente mediante el uso de FTP). Si realiza una copia manual, también tendrá que copiar los archivos que admiten SQL Server Compact (la base de datos). Para obtener más información, vea la entrada de blog [aplicaciones de implementación de las páginas Web sin necesidad de una herramienta](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>¿Tengo que usar la aplicación auxiliar WebSecurity para admitir los inicios de sesión?

No. El `SimpleMembership` proveedor que forma parte de las páginas Web ASP.NET es una opción. Los proveedores de seguridad que forman parte de ASP.NET (que podría usarse para trabajar con en formularios Web Forms) también están disponibles. Por ejemplo, se puede utilizar la autenticación de formularios en ASP.NET Web Pages igual que haría en formularios Web Forms. Para un ejemplo de cómo usar la autenticación de formularios, vea el artículo de Microsoft Support [How To Implement Forms-Based la autenticación en la aplicación de ASP.NET mediante el uso de C# .NET](https://support.microsoft.com/kb/301240). Para descargar un ejemplo sencillo, consulte [versión de ASP.NET de "inicio de sesión &amp; contraseña](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Para obtener información sobre cómo usar la autenticación de Windows, consulte la entrada de blog [en ASP.NET Web Pages de la autenticación de Windows usando](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>¿Admite ASP.NET Web Pages HTML5?

Sí. Las páginas que se creen con ASP.NET Web Pages (*.cshtml* o *.vbhtml* páginas) son básicamente las páginas HTML que también contienen código que se ejecuta en el servidor, antes de presentar la página. Siempre que el explorador del usuario es compatible con HTML5, puede usar elementos de HTML5 en un *.cshtml* o *.vbhtml* página.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>¿Puedo usar JavaScript y jQuery con páginas Web?

Por supuesto. Las páginas que se creen con ASP.NET Web Pages (*.cshtml* o *.vbhtml* páginas) son simplemente las páginas HTML con código del servidor en ellos. Por lo tanto, cualquier cosa que puede hacer en una página HTML normal usando JavaScript o jQuery también puede hacer un *.cshtml* o *.vbhtml* página.

El **Starter Site** plantilla en WebMatrix contiene un número de bibliotecas de jQuery. Si crea un sitio mediante el uso de esa plantilla, el *Scripts* carpeta contiene una biblioteca de núcleo de jQuery (*1.6.2.js jquery)* y bibliotecas para la validación de jQuery (*jquery.validate.js*, etcetera.).

Estas son algunas entradas de blog que muestran las formas de usar jQuery con ASP.NET Web Pages:

- [Adición de jQuery adecuación a ASP.NET Web Pages mediante WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) por Rachel Appel
- [5 minutos: WebMatrix jQuery UI json + + jQuery plantillas](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) por Jonas Eriksson
- [WebMatrix y formularios de jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) por Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Guía de solución de problemas (Razor) de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253001)

[Foro de WebMatrix y ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) en el sitio Web ASP.NET
